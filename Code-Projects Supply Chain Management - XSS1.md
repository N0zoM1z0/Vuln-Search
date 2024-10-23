# Summary
There is a cross site scripting (XSS) vulnerability in code-projects supply chain management (https://code-projects.org/supply-chain-management-in-php-css-js-and-mysql-free-download/).

Affected component is `/retailer/edit_profile.php`.

# Details
In `/retailer/edit_profile.php`, we fill the `Address` blank with XSS payload as follows:
```html
<img/src="x"/onerror=alert(`xss`);>
```
![image](https://github.com/user-attachments/assets/63bf014d-5a7b-4712-b1ba-2ccc2916b3fe)

We update profile and return to `/retailer/index.php`, finding that XSS is triggered:
![image](https://github.com/user-attachments/assets/8baf65fa-a842-4a73-8db5-c461b8b8bae3)

Corresponding HTML page:
![image](https://github.com/user-attachments/assets/349eb4cc-d2ac-4c88-a86b-802524bdc503)



# PoC
```html
<img/src="x"/onerror=alert(`xss`);>
```

![image](https://github.com/user-attachments/assets/63bf014d-5a7b-4712-b1ba-2ccc2916b3fe)

![image](https://github.com/user-attachments/assets/8baf65fa-a842-4a73-8db5-c461b8b8bae3)
