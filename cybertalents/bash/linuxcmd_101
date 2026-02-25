# 🐧 Linux 1o1 -- CTF Write-Up

**Category:** Bash\
**Difficulty:** Medium\
**Challenge Name:** Linux 1o1\
**Challenge File:**\
https://dr-hubchallenges.s3.us-west-2.amazonaws.com/Forensics/linux_chal.tar.gz

------------------------------------------------------------------------

## 📌 Overview

This challenge walks through multiple layers of compressed files,
password-protected archives, encoding techniques, and basic Linux
enumeration skills.

It tests familiarity with:

-   Archive extraction (`gzip`, `tar`, `zip`)
-   Hidden files
-   Wordlists & brute-forcing
-   Base64 decoding
-   ROT13 encoding
-   Basic file inspection tools

------------------------------------------------------------------------

# 🧩 Solution Walkthrough

------------------------------------------------------------------------

## 🔹 Step 1 -- Extract the Initial Archive

We are provided with:

    linux_chal.tar.gz

This is a standard gzip archive.

``` bash
gzip -d linux_chal.tar.gz
```

This gives us:

    linux_chal.tar

------------------------------------------------------------------------

## 🔹 Step 2 -- Extract the TAR Archive

``` bash
tar -xvf linux_chal.tar
```

This produces a directory named:

    cat/

------------------------------------------------------------------------

## 🔹 Step 3 -- Discover Hidden Password

Inside `cat/`, we find a password-protected ZIP file.

Listing hidden files:

``` bash
ls -lsa
```

We discover a hidden file:

    .pass.txt

Inside it, we find the password:

    2434237800

We use it to extract the ZIP archive.

------------------------------------------------------------------------

## 🔹 Step 4 -- Executing a Suspicious Binary

After extraction, we get:

-   `ascii.zip`
-   A binary file named `-`

The file named `-` can be executed using:

``` bash
./-
```

This reveals the password for `ascii.zip`:

    passforasciiii

------------------------------------------------------------------------

## 🔹 Step 5 -- ASCII File Analysis

Inside the extracted `ascii/` directory we find:

    f0  f1  f2  f3  f4  f5  f6  f7  f8
    size37.zip

Checking file types:

``` bash
file *
```

We identify that `f6` is ASCII text.

Reading it reveals the password:

    thisisasciiiiiprintapleeeee

We use it to extract:

``` bash
unzip size37.zip
```

------------------------------------------------------------------------

## 🔹 Step 6 -- Wordlist Brute Force with John the Ripper

Inside `size37/` we find:

-   `next.zip`
-   `test1` → `test7` (ASCII files)

Each `test` file contains encoded-looking text. When combined, they form
a wordlist.

### Create the wordlist:

``` bash
cat test* > wordlist
```

### Extract ZIP hash:

``` bash
zip2john next.zip > hash
```

### Brute force with:

``` bash
john -w=wordlist hash
```

We successfully crack the password and extract `next.zip`.

------------------------------------------------------------------------

## 🔹 Step 7 -- Hint in File Name

Inside `next/`:

-   `nexttocybertalents`
-   `NumberOne.zip`

The filename `nexttocybertalents` is a hint.

The word *next to* `cybertalents` is:

    orderby1337

That is the password for `NumberOne.zip`.

------------------------------------------------------------------------

## 🔹 Step 8 -- Another John Attack

Inside the extracted folder:

-   `one` (wordlist)
-   `decodeme1.zip`

Again:

``` bash
zip2john decodeme1.zip > hash
john -w=one hash
```

Password is recovered successfully.

------------------------------------------------------------------------

## 🔹 Step 9 -- Base64 Decoding

Inside the next directory:

-   `pass`
-   `decodeme2.zip`

The file `pass` contains:

    dXNlbWVhc3Bhc3N3b3Jk

This is Base64 encoded.

Decode it:

``` bash
echo "dXNlbWVhc3Bhc3N3b3Jk" | base64 -d
```

Output:

    usemeaspassword

Use it to extract `decodeme2.zip`.

------------------------------------------------------------------------

## 🔹 Step 10 -- ROT13 Final Decode

We obtain:

    flag.txt

The contents appear obfuscated.

It is encoded using ROT13.

Decode using:

``` bash
cat flag.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

This reveals the final flag.

------------------------------------------------------------------------

# 🏁 Final Flag

    flag{s1mple_redacted_101}

------------------------------------------------------------------------

# 🧠 Skills Demonstrated

-   Archive extraction (gzip, tar, zip)
-   Hidden file discovery
-   File type inspection
-   Executing Linux binaries
-   Wordlist creation
-   ZIP hash extraction
-   Brute forcing with John the Ripper
-   Base64 decoding
-   ROT13 decoding
-   Basic enumeration methodology
