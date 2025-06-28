# Encrypted Database - Web Challenge Writeup

## Challenge Details
| **Category** | **Name**            | **Level** | **Points** | **Challenge Link**                          |
|--------------|---------------------|-----------|------------|---------------------------------------------|
| Web          | Encrypted Database  | Easy      | 50         | [View Challenge](https://cybertalents.com/challenges/web/encrypted-database) |

## Challenge Description
> "The company hired an inexperienced developer, but he told them he hided the database and have it encrypted so the website is totally secure, can you prove that he is wrong?"

## Solution Walkthrough

### 1. Initial Reconnaissance
- Started by examining the static website which appeared normal
- Conducted directory brute-forcing using Gobuster:
  ```bash
  gobuster dir -u http://target.com -w /path/to/wordlist.txt
  ```

  
  2. Discovery

    Found /admin/ directory

    Attempted basic credentials (admin:admin)

    Noticed error message appeared before credential submission


  3. Source Code Analysis

    Inspected page source (Ctrl+U)

    Discovered exposed database path in client-side code

    Example vulnerability:
   <!-- Hidden comment exposing sensitive path -->
   <!-- DB backup at /backups/db.enc -->


  4. Exploitation

    Accessed exposed database path: http://target.com/backups/db.enc

  Retrieved encrypted flag


  5. Hash Cracking

    Used Hashes.com to crack the encryption

    Identified hash type (e.g., MD5, SHA1)

    Obtained plaintext flag: bad(redacted)

  

