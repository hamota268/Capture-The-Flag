# Mob Psycho - picoCTF 2026

**Category:** Forensics  
**Difficulty:** Medium  
**Challenge File:** [mobpsycho.apk](https://artifacts.picoctf.net/c_titan/51/mobpsycho.apk)

## Description
Can you handle APKs? Download the android apk and find the flag.

## Solution

**Step 1: Download the APK**  
First, get the APK file using wget:

```bash
wget https://artifacts.picoctf.net/c_titan/51/mobpsycho.apk
```

Once downloaded, the file is an APK (Android package) file. I started with the basics by searching for strings that might contain the flag:

```bash
strings mobpsycho.apk | grep -e 'pico' -e 'flag'
```

Nothing was found.

---

**Step 2: Explore the APK as an archive**  
Instead of decompiling, I treated the APK as a standard archive since APKs are zip-based. I extracted it with:

```bash
unzip mobpsycho.apk -d unpacked
```

Inside the `unpacked` directory, I searched for files related to flags:

```bash
tree | grep flag
```

I found a file called `flag.txt` located in `unpacked/res/color`. Opening the file revealed what looked like gibberish:

```
7069636f4354467b6178386d433052553676655f4e5838356c346178386d436c5f35326135653264657d
```

---

**Step 3: Decode the flag**  
Using an online tool like [dcode.fr](https://www.dcode.fr/hexadecimal-ascii), I converted the hexadecimal string to ASCII, which revealed the flag.

---

**Flag:**  

```
picoCTF{ax8mC0RU6ve_NX85l4ax8mCl_redacted}
```
