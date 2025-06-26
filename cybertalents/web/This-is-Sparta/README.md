# This is Sparta - CTF Challenge Write-Up

![CTF Category: Web]
![Difficulty: Easy] 
![Points: 50]

## Challenge Overview
- **Challenge Name**: This is Sparta
- **Challenge Link**: https://cybertalents.com/challenges/web/this-is-sparta
- **Description**: 
  > "Morning has broken today they're fighting in the shade when arrows blocked the sun they fell tonight they dine in hell"

## Solution Walkthrough

### Step 1: Initial Analysis
1. **Accessed the website**:
   - Found a basic login form
   - Discovered a "Hint" button that displayed: 
     ```
     Easier than Ableton
     ```
   *(Note: This suggests client-side validation, referencing the movie "300" where Spartan warriors fought in shade of arrows)*

2. **Tested default credentials**:
   - Attempted `admin:admin` → No success
   - No cookies present in application storage

### Step 2: Source Code Investigation
1. **Inspected page source** (Right-Click → View Page Source):
   - Found obfuscated JavaScript attached to the login button:
     ```javascript
     function _0x3a12(_0xeb80x2,_0xeb80x3){var _0x3a1228... // [truncated]
     ```

### Step 3: JavaScript Deobfuscation
1. **Used online deobfuscator** (e.g., [deobfuscate.io](https://deobfuscate.io/)):
   - Revealed the clean authentication logic:
     ```javascript
     function check() {
       var username = document.getElementById("user").value;
       var password = document.getElementById("pass").value;
       
       if (username == "Cyber-Talent" && password == "Cyber-Talent") {
         alert("Congratz \n\nFlag: redacted");
       } else {
         alert("wrong Password");
       }
     }
     ```

### Step 4: Flag Retrieval
1. **Entered credentials**:
   - Username: `Cyber-Talent`
   - Password: `redacted`
2. **Received flag** via alert popup: flag{redacted}
