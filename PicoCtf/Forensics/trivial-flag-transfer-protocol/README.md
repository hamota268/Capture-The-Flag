# 📡 Trivial Flag Transfer Protocol -- CTF Write-Up

**Category:** Forensics\
**Difficulty:** Medium\
**Challenge Name:** Trivial Flag Transfer Protocol\
**Challenge File:**\
https://challenge-files.picoctf.net/c_wily_courier/9f32bef3f1ad1e5a7c5992f3c1e86619ff557537080e163e5a6d9f01070192a9/tftp.pcapng

------------------------------------------------------------------------

## 📌 Overview

This challenge focuses on network traffic analysis and file extraction
from a packet capture file.\
The objective is to determine how the flag was transferred using the
TFTP protocol and uncover the hidden data.

------------------------------------------------------------------------

# 🧩 Solution Walkthrough

------------------------------------------------------------------------

## 🔹 Step 1 -- Obtain the PCAP File

We are provided with a `.pcapng` (Next Generation Capture) file from the
challenge link.

------------------------------------------------------------------------

## 🔹 Step 2 -- Analyze with Wireshark

Open the capture file using Wireshark.

Upon inspection, we observe:

-   Significant TFTP traffic
-   Some ARP traffic
-   The IP `10.10.10.12` requesting a file named `instructions.txt`

This indicates files are being transferred over TFTP.

------------------------------------------------------------------------

## 🔹 Step 3 -- Export TFTP Objects

To retrieve transferred files:

1.  Go to **File → Export Objects → TFTP**
2.  Save all extracted files

We obtain:

-   `instructions.txt`
-   `plan`
-   `program.deb`
-   `picture1.bmp`
-   `picture2.bmp`
-   `picture3.bmp`

------------------------------------------------------------------------

## 🔹 Step 4 -- Decode instructions.txt

The contents of `instructions.txt`:

    GSGCQBRFAGRAPELCGBHEGENSSVPFBJRZHFGQVFTHVFRBHESYNTGENAFSRE.SVTHERBHGNJNLGBUVQRGURSYNTNAQVJVYYPURPXONPXSBEGURCYNA

This appears to be ciphertext.

After identifying it as ROT13, decoding gives:

    TFTP DOESNT ENCRYPT OUR TRAFFIC SO WE MUST DISGUISE OUR FLAG TRANSFER.
    FIGURE OUT A WAY TO HIDE THE FLAG AND I WILL CHECK BACK FOR THE PLAN

This references a file named `plan`.

------------------------------------------------------------------------

## 🔹 Step 5 -- Decode the Plan

The `plan` file also contains ROT13-encoded text.

After decoding:

    I USED THE PROGRAM AND HID IT WITH-DUEDILIGENCE.
    CHECK OUT THE PHOTOS

This gives two important hints:

-   A program was used
-   The flag is hidden in the photos
-   Keyword: DUEDILIGENCE

------------------------------------------------------------------------

## 🔹 Step 6 -- Investigate program.deb

The file `program.deb` is a Debian package.

Extract it using:

``` bash
dpkg-deb -x program.deb ./extract-file
```

Inspect the contents:

``` bash
tree extract-file/usr
```

We find a tool called:

    steghide

This suggests steganography was used.

------------------------------------------------------------------------

## 🔹 Step 7 -- Attempt Steganography Extraction

From the extracted README, we find usage instructions like:

``` bash
steghide extract -sf picture.jpg
```

We attempt extraction on the BMP files.

Start with:

``` bash
steghide extract -sf picture2.bmp
```

It prompts for a password.

------------------------------------------------------------------------

## 🔹 Step 8 -- Identify the Password

From the decoded `plan` file:

    I USED THE PROGRAM AND HID IT WITH-DUEDILIGENCE

The word DUEDILIGENCE appears to be the password.

Trying:

``` bash
steghide extract -sf picture3.bmp
```

Password:

    DUEDILIGENCE

Successful extraction:

    wrote extracted data to "flag.txt"

------------------------------------------------------------------------

## 🔹 Step 9 -- Retrieve the Flag

Opening `flag.txt` reveals:

    picoCTF{h1dd3n_1n_pLa1n_51GHT_redacted}

------------------------------------------------------------------------

# 🏁 Final Flag

    picoCTF{h1dd3n_1n_pLa1n_51GHT_redacted}

------------------------------------------------------------------------

# 🧠 Skills Demonstrated

-   Network traffic analysis with Wireshark
-   TFTP object extraction
-   ROT13 cipher decoding
-   Debian package extraction
-   Steganography detection and extraction
-   Logical investigation using contextual hints

------------------------------------------------------------------------

## 💡 Key Takeaway

Even if a protocol does not encrypt traffic (like TFTP), attackers may
still attempt to conceal sensitive data using steganography.\
Careful traffic inspection and logical reasoning are essential in
forensic investigations.
