# Cryptography Challenge: interencdec (Easy)

## Challenge Description
**Challenge Name:** interencdec  
**Challenge Level:** Easy  
**Challenge File:** [enc_flag](https://artifacts.picoctf.net/c_titan/1/enc_flag)  

**Description:**  
Can you get the real meaning from this file?

## Solution

### Overview
This challenge involves decrypting a flag that has been encoded with multiple layers of transformation, including Base64 encoding and Caesar cipher encryption.

### Step-by-Step Solution

1. **Download the File:**
   ```bash
   wget https://artifacts.picoctf.net/c_titan/1/enc_flag

2. **Initial Inspection:**
The file contains what appears to be Base64 encoded data:
  "YidkM0JxZGtwQlRYdHFhR3g2YUhsZmF6TnFlVGwzWVROclgyeG9OakJzTURCcGZRPT0nCg=="

3. **First Base64 Decode:**
     echo "YidkM0JxZGtwQlRYdHFhR3g2YUhsZmF6TnFlVGwzWVROclgyeG9OakJzTURCcGZRPT0nCg==" | base64 -d

   output: b'd3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrX2xoNjBsMDBpfQ=='

4. **Second Base64 Decode:**
    Remove the b' and ' characters and decode again (using CyberChef or command line):
     echo "d3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrX2xoNjBsMDBpfQ==" | base64 -d

      output: wpjvJAM{jhlzhy_k3jy9wa3k_lh60l00i}
   
5. **Caesar Cipher Decryption:**
    The text appears to be encrypted with a Caesar cipher. After testing rotations:

    ROT3 reveals the flag format

    Alternatively, use the caesar command (from bsdgames package):
     echo "wpjvJAM{jhlzhy_k3jy9wa3k_lh60l00i}" | caesar
     output: echo "wpjvJAM{jhlzhy_k3jy9wa3k_lh60l00i}" | caesar

5. **One-Liner Solution:**   
     ```bash
     cat enc_flag | base64 -d | cut -d "'" -f2 | base64 -d | caesar

Flag: picoCTF{caesar_d3cr9pt3d_redacted}     
     
