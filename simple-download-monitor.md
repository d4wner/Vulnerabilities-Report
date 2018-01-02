Well ,sir ,I just found a Stored-XSS bug here.

When I login into the wordpress panel, assume I have a low privilege role like a contributor user.

Because the admin user has turned on the option of the wp-plugin  simple-download-monitor, a normal user like me can also use it.

Now I can write something in the function "Edit Download":
```
http://localhost/wordpress/wp-admin/post.php?post=x&action=edit
```

But when I fuzz the parameters in this plugin, I found when I write something into these points, it does not filter well:
```
1. File Thumbnail (Optional)
2. Downloadable File (Visitors will download this item)
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/simple-download-monitor/sxss1.png)
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/simple-download-monitor/sxss2.png)


While it tell us to enter a valid URL of the file in the text box below, I can write something with evil content like:

```
http://www.test.com/1.php'"><svg/onload=alert(document.cookie)><'"
```

Then we can publish the post or just submit it to the admin user for an audit.

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/simple-download-monitor/sxss4.png)

It won't be long beofore I get the other user's cookie or do something more evilly.

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/simple-download-monitor/sxss3.png)
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/simple-download-monitor/sxss5.png)

Well,  by the way, I just test the bug in the wordpress 4.9.1 and the latest version of the wp-plugin simple-download-monitor.