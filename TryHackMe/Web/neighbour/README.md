Challenge Name: Neighbour
Category: Web
Difficulty: Easy
Link: TryHackMe Room
Challenge Description

The challenge presents a simple login form with some hidden clues in the source code.
Solution Walkthrough

  1.Initial Investigation:

        Upon accessing the machine, we're presented with a basic login form.

        Checking the page source (Ctrl+U) reveals an important HTML comment:
          <!-- if you don't have an account use the guest account and the admin is offlimits -->

  2.Guest Login:

    Using the hint, we log in with the guest account credentials.

    After successful login, we notice two important elements:

        A logout button

        Our username displayed in the URL (e.g., ?user=guest)
 
  3.Privilege Escalation:

    The URL parameter appears to be vulnerable to direct object reference.

    By changing the URL from ?user=guest to ?user=admin, we bypass the intended access controls

  4.Flag Retrieval:

    After accessing the admin view, the flag is displayed on the page.


  Flag:
    flag{66be95c478473d91a5358fredacted}


    
