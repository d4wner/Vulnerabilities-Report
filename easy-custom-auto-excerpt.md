Well ,sir ,I just found a Stored-XSS bug here.

The report link to the wordpress-form is missing, because the manager do not wish to put the public in danger ,I'll just write some details here.

When I login into the wordpress panel, assume I have a low privilege role like a editor user.

Because the admin user has turned on the option of the wp-plugin  Easy Custom Auto Excerpt, a normal user like me can also use it.

When I edit the setting page of Excerpt, I write something evil into it:

```
http://192.168.1.109/wordpress/wp-admin/admin.php?page=tonjoo_excerpt
```

Weak post para:

```
tonjoo_ecae_options%5Bcustom_css%5D='"><img src=x onerror=console.log(/xss/)><'"
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/easy-custom-auto-excerpt/sxss1.png)

Once the other users or the manager view the page , I'll get the cookies of theirs , or do something more evilly.

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/easy-custom-auto-excerpt/sxss2.png)
