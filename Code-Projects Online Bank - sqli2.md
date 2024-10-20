# Summary
There is a sql injection vulnerability in code-project Online Bank (https://code-projects.org/online-bank-in-php-css-javascript-and-mysql-free-download/).

Affected component is `/send_funds_action.php`.

# Details
In `send_funds_action.php?cust_id=1`,
parameter `cust_id` is vulnerable to sql injection, 
![image](https://github.com/user-attachments/assets/21c8c449-0e04-401e-89c8-c87337abf8fa)

we can use `sqlmap` to test:
![image](https://github.com/user-attachments/assets/c85ddd52-0e2f-4bd9-ab32-d715c08687b1)



Audit the code, in `send_funds_action.php`, there is no filter for `$_GET['cust_id']`:
![image](https://github.com/user-attachments/assets/9f5f4228-a486-4f94-a329-10803aeb273b)

and these parameters are directly spliced into `$sql0`, which leads to sql injection vulnerability.



# PoC
![image](https://github.com/user-attachments/assets/c85ddd52-0e2f-4bd9-ab32-d715c08687b1)
