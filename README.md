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
