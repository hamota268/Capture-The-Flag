# Forgotten Ruins -- OSINT Challenge Write-up

**Category:** OSINT\
**Platform:** vulnbydefault

This challenge revolves around digital footprint analysis and
demonstrates how publicly exposed information can be pieced together to
reconstruct sensitive credentials. The objective is to obtain valid
login credentials by leveraging open-source intelligence techniques.

------------------------------------------------------------------------

## Service Overview

After connecting to the target service using `nc`, three options are
presented:

1.  Login\
2.  Forgot Password\
3.  Exit

Since no credentials are initially available, attempting to log in is
not practical. The logical starting point is analyzing the **Forgot
Password** functionality.

------------------------------------------------------------------------

## Password Reset Message Analysis

Submitting a valid email address triggers a password reset message. The
key excerpt from the email states:

> "Company policy requires passwords to contain three important things
> in your life."

The email is signed by:

Owen Rosz\
Owenrosz@vbd.com\
Support Team

This immediately provides:

-   A full name\
-   A likely username format\
-   Insight into the password construction logic

The wording strongly implies that the password consists of three
personally meaningful elements.

------------------------------------------------------------------------

## Intelligence Gathering

The next phase involved enumerating public information linked to the
name **Owen Rosz** across social platforms.

An Instagram profile was identified, revealing several notable
references:

-   Istanbul\
-   Turkey\
-   Taksim Square\
-   The handle "cuteomencat"

These details suggest possible password components, especially when
considering common personal password patterns such as:

-   Pet names\
-   Favorite locations\
-   Significant dates

The investigation continued by analyzing the "cuteomencat" account.

------------------------------------------------------------------------

## Secondary Account Investigation

The "cuteomencat" username led to a Reddit account. There, a date of
birth was listed as:

-   7 September 2013 (edited)

Attempts were made to retrieve historical edits or archived data, but
this did not produce actionable results. Therefore, attention shifted
back to other clues from social media content.

------------------------------------------------------------------------

## Numeric Hint -- "Leet"

One post contained the following statement:

> "I love my house number, it's literally leet. Remember it's numbers
> only."

In cybersecurity culture, "leet" translates to:

**1337**

This became a strong candidate for one of the required password
components.

At this point, additional effort was spent performing image-based
analysis. Visual indicators suggested locations within Iran (notably
Tehran), based on Persian text and regional formatting patterns.
Alternative mapping services such as Balad and Neshan were explored due
to limited Google Street View coverage.

Although this geolocation phase turned out to be unnecessary for solving
the challenge, it reinforced practical OSINT investigation skills.

------------------------------------------------------------------------

## Constructing the Password Model

By consolidating findings, the likely password structure appeared to
follow this format:

{component1}{Omen}{component3}

Probable components included:

-   1337 (derived from "leet")\
-   Omen (associated with cuteomencat)\
-   A numeric value (likely a year or date)

Since password policies frequently end with numbers, testing
combinations with numeric suffixes became the priority.

------------------------------------------------------------------------

## Efficient Testing Strategy

Due to time constraints imposed by the challenge environment, repeatedly
reconnecting for each attempt was inefficient. To optimize the process:

-   The wordlist was divided into smaller segments\
-   A single persistent connection was maintained\
-   Multiple credential attempts were sent sequentially without closing
    the session

This approach significantly reduced overhead and improved testing speed.

Eventually, the correct credential combination was discovered:

**1337Omen2003**

------------------------------------------------------------------------

## Flag

**VBD{OSINTk1ng_f0r_A\_redacted}**
