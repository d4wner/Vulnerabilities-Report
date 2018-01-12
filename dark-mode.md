Well ,sir ,I just found some Stored-XSS bugs at wp-plugin dark-mode.

#### Stored-XSS

When I visit the user profile page as a normal user contributor, I'll see the dark-mode function here:

```
http://127.0.0.1/wordpress/wp-admin/profile.php
```


But when I pentest the  parameter in this plugin, I found when I write something into this point, it does not filter well.

Weak data parameter:

```
1.dark_mode_start='"><svg/onload=console.log(/xss1/)><'"
2.dark_mode_end='"><svg/onload=console.log(/xss2/)><'"
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/dark-mode/sxss1.png)


When the managers login into the panel, if they edit the profile page of contributor, I can get their cookie easily, or do something more evilly.

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/dark-mode/sxss2.png)

Well,  by the way, I just test the bug in the wordpress 4.9.1 and the latest version of the wp-plugin dark-mode.