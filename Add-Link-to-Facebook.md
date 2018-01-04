Well ,sir ,I just found a Stored-XSS bug here.

When I login into the wordpress panel, assume I have a low privilege role like a editor user.

```
http://localhost/wordpress/wp-admin/users.php
```
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Add-Link-to-Facebook/roles.png)

Because the admin user has turned on the option of the wp-plugin  Add-Link-to-Facebook, a normal user like me can also use it.

I can write something in the profile page.
By the way, I'm the user "test" now:
```
http://localhost/wordpress/wp-admin/profile.php
```

But when I pentest the  parameter in this plugin, I found when I write something into this point, it does not filter well:

```
al2fb_facebook_id='"><img src=x onerror=console.log(/xss_att_fackbookid/)><'"
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Add-Link-to-Facebook/insert-xss.png)



While it tell us to enter a valid URL of the file in the text box below, I can write something with evil content like:

```
http://www.test.com/1.php'"><svg/onload=alert(document.cookie)><'"
```

Then we can publish the post or just submit it to the admin user for an audit.
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Add-Link-to-Facebook/view-xss.png)

It won't be long beofore I get the other user, even the admin user's cookie or do something more evilly.


Well,  by the way, I just test the bug in the wordpress 4.9.1 and the latest version of the wp-plugin Add-Link-to-Facebook.