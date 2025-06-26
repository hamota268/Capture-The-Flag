CTF Challenge Write-Up: Web Exploitation
Challenge Name: Admin Has The Power

Category: Web
Level: Easy
Points: 50
Challenge Link: http://wcamxwl32pue3e6m5p6v4ehxzg1r4nzy8yq4hnzk-web.cybertalentslabs.com/
Challenge Description

    "Administrators only have the power to see the flag. Can you be one?"

Solution Walkthrough
Step 1: Initial Reconnaissance

    Access the Website:

        Open the provided URL in a browser.

        A login form appears, requesting a username and password.

    Test Default Credentials:

        Attempt common logins (e.g., admin:admin, admin:password).

        Observe that admin:admin does not work, but the response suggests cookie-based authentication.

Step 2: Source Code Inspection

    Check Page Source (Ctrl+U / Right-Click → View Source):

        Find a hidden comment containing credentials:
        html

<!-- TODO: Remove before production! 
     Support credentials: support / redacted -->

Use these credentials to log in:
text

        Username: support  
        Password: redacted  

Step 3: Cookie Tampering Attack

    Analyze Cookies:

        After logging in, check browser cookies (F12 → Application → Cookies).

        Notice a cookie like:
        text

    role=support  

Modify Cookie for Privilege Escalation:

    Using a cookie editor extension (e.g., "EditThisCookie" for Chrome), change:
    diff

        - role=support  
        + role=admin  

        Refresh the page.

Step 4: Capture the Flag

    Flag Retrieval:

        After tampering, the page grants admin access, revealing the flag.

Attack Explanation

This challenge demonstrates Cookie Tampering, where modifying client-side session data (cookies) allows privilege escalation. The web app trusted the role cookie value without proper server-side validation.
Prevention

    Secure Cookies: Use HttpOnly, Secure, and SameSite flags.

    Server-Side Validation: Never trust client-side inputs; validate roles server-side.

    Session Management: Use signed/encrypted cookies (e.g., JWT with signatures).

Final Notes

✅ Difficulty: Easy (Basic web exploitation)
✅ Key Techniques: Source code review, cookie manipulation
✅ Tool Used: Browser DevTools, Cookie Editor Extension
