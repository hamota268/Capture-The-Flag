# Cryptography Challenge: Hashcrack (Easy)

## Challenge Description
**Challenge Name:** Hashcrack  
**Challenge Level:** Easy  
**Host:** `verbal-sleep.picoctf.net:62644`  

**Description:**  
A company stored a secret message on a server which got breached due to the admin using weakly hashed passwords. Can you gain access to the secret stored within the server?

## Solution

### Overview
This challenge involves cracking a series of weakly hashed passwords to reveal a hidden flag. The solution demonstrates the dangers of using weak hashing algorithms and common passwords.

### Step-by-Step Solution

1. **Connect to the Service:**
   Upon connecting to the service, you're presented with a hash:
     Welcome!! Looking For the Secret?
     We have identified a hash: 482c811da5d5b4bc6d497ffa98491e38


2. **Identify the Hash Algorithm:**
Use `hashid` to identify the hashing algorithm:
```bash
hashid 482c811da5d5b4bc6d497ffa98491e38
```
Output suggests this is an MD5 hash, which is cryptographically weak and susceptible to cracking.

3. **Crack the First Hash:**
Using an online hash cracking service like Hashes.com:

    Input: 482c811da5d5b4bc6d497ffa98491e38

    Result: password123

   
4. **Submit the Password:**
When submitted, the service returns another hash:
  b7a875fc1ea228b9061041b7cec4bd3c52ab3ce3

    This is identified as SHA-1 (also weak).

    Cracked result: letmein


5. **Final Hash:**
Submitting the second password reveals a third hash:
  916e8c4f79b25028c9e467f1eb8eee6d6bbdff965f9928310ad30a8d88697745

    This is identified as SHA-256.

    Cracked result: qwerty098

6. **Retrieve the Flag:**
After submitting the third password, the service reveals the flag:
  picoCTF{UseStr0nG_h@shEs_&PaSswDs!_redacted}

