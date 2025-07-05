# Forensics Challenge: Disko 1 (Easy)

## Challenge Description
**Challenge Name:** Disko 1  
**Challenge Level:** Easy  
**Challenge Link:** [Download disk image](https://artifacts.picoctf.net/c/538/disko-1.dd.gz)  

**Description:**  
Can you find the flag in this disk image? Download the disk image here.

## Solution

### Overview
The challenge provides a compressed disk image that contains a hidden flag. The solution involves analyzing the disk image to extract the flag without mounting it.

### Step-by-Step Solution

1. **Download the Disk Image:**
   ```bash
   wget https://artifacts.picoctf.net/c/538/disko-1.dd.gz
    ```
    
2. **Uncompress the File:**
    The downloaded file is compressed with gzip. Uncompress it using: 
      ```bash
      gzip -d disko-1.dd.gz
      ```
      This will extract the disk image file disko-1.dd.

3. **Analyze the Disk Image:**
   The file is a FAT filesystem image. You can verify this using the file command: 
     ```bash
     file disko-1.dd
     ```

     **output:**
         disko-1.dd: DOS/MBR boot sector, code offset 0x58+2, OEM-ID "mkfs.fat",
         Media descriptor 0xf8, sectors/track 32, heads 8, sectors 102400 (volumes > 32 MB),
         FAT (32 bit), sectors/FAT 788, serial number 0x241a4420, unlabeled

4. **Search for the Flag:**
Instead of mounting the disk image, we can directly search for the flag using the strings command combined with grep:
    ```bash
         strings disko-1.dd | grep -i "picoCTF"
    ```

5. **Retrieve the Flag:**
The command above will output the flag:  
   picoCTF{1t5_ju5t_4_5tr1n9_redacted}
