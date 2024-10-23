# Summary
There is a sql injection vulnerability in code-projects supply chain management (https://code-projects.org/supply-chain-management-in-php-css-js-and-mysql-free-download/).

Affected component is `/admin/view_invoice_items.php`.

# Details
In `/admin/view_invoice_items.php?id=1`, parameter `id` is vulnerable to sql injection:
![image](https://github.com/user-attachments/assets/7a9eda81-fde4-4e91-af55-33dc3699b63b)

Apparently, there is no filter for `$_GET['id']`, which leads to sql injection vulnerability.

We can use `sqlmap` to test:
![image](https://github.com/user-attachments/assets/1c519d24-a37b-446d-b1a0-49577f5b2e96)


# PoC
We can use `sqlmap` to test.
Firstly, use burpsuite to intercept traffic packets, copy it in `post.txt` and then
```
sqlmap -r post.txt --dbs --batch
```

![image](https://github.com/user-attachments/assets/1c519d24-a37b-446d-b1a0-49577f5b2e96)
