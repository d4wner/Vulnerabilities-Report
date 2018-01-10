Well ,sir ,I just found a Stored-XSS bug here.

The report link to the wordpress-form is missing, because the manager do not wish to put the public in danger ,I'll just write some details here.

When I login into the wordpress panel, assume I have a low privilege role like a editor user.

Because the admin user has turned on the option of the wp-plugin  tabs-responsive, a normal user like me can also use it.

When I edit the setting page of tabs-responsive, I write something evil into it:

Firstly, I should click the url ,and edit some thing in the single page:

```
http://localhost/wordpress/wp-admin/post.php?post=x&action=edit
```
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/tabs-responsive/page.png)

Then I should save the changes:

Post url；

```
http://localhost/wordpress/wp-admin/post.php
```

Weak post para:

```
post_title=<script>console.log(/xss/)</script>
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/tabs-responsive/sxss1.png)

Once the other users or the manager view the edit page , I'll get the cookies of theirs , or do something more evilly.

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/tabs-responsive/sxss2.png)
