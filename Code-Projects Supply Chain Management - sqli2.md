# Summary
There is a sql injection vulnerability in code-projects supply chain management (https://code-projects.org/supply-chain-management-in-php-css-js-and-mysql-free-download/).

Affected component is `/admin/edit_retailer.php`.

# Details
In `/admin/edit_retailer.php?id=1`, parameter `id` is vulnerable to sql injection:
![image](https://github.com/user-attachments/assets/2af5e6ca-4299-45b1-a529-92736901aee6)

Apparently, there is no filter for `$_GET['id']`, which leads to sql injection vulnerability.

We can use `sqlmap` to test:
![image](https://github.com/user-attachments/assets/95d1a42a-b789-4a63-b0b6-bb466ec55ccc)


# PoC
We can use `sqlmap` to test.
Firstly, use burpsuite to intercept traffic packets, copy it in `post.txt` and then
```
sqlmap -r post.txt --dbs --batch
```

![image](https://github.com/user-attachments/assets/95d1a42a-b789-4a63-b0b6-bb466ec55ccc)
