### Readymade Classifieds Script- the lastest version - SQL Injection/XSS/Unauthorized access

Well,  when I pentest the official demo site of Readymade Classifieds Script, I found some vulnerabilities here.

```
http://live.posty.in/admin/user_activate_submit.php
```

#### Unauthorized access:

For example, this link is an backend link ,which should not be viewed by visitors, this vulnerability could leak some sensitive Information or be exploit by evil user.

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Readymade-Classifieds-Script/access.png)

#### XSS && SQLi:

And  SQL Injection and XSS vulnerability also exist here:

##### xss:

http://live.posty.in/admin/user_activate_submit.php?ID=457%27%22%3E%3Csvg/onload=alert(/xss/)%3E%3C%27%22


![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Readymade-Classifieds-Script/xss.png)


##### SQL Injection:


![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Readymade-Classifieds-Script/sqli.png)


You can see,  we can obtain the current data user or more sensitive data now!


