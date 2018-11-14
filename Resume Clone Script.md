### Resume Clone Script- the lastest version - SQL Injection

#### ADLab of Venustech

Well,  when I pentest the official demo site of Resume Clone Script, I found some vulnerabilities here.


#### SQL Injection:

```
http://www.jobportalscript.com/resume-builder-script/forget.php
```

post parameter:
```
------WebKitFormBoundaryMBbGcdUZDV0hA2XK
Content-Disposition: form-data; name="username"

123
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Resume-Clone-Script/sqli.png)


You can see,  we can obtain the current data user or more sensitive data now!
