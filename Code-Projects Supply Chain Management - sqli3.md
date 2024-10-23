# Summary
There is a sql injection vulnerability in code-projects supply chain management (https://code-projects.org/supply-chain-management-in-php-css-js-and-mysql-free-download/).

Affected component is `/admin/edit_area.php`.

# Details
In `/admin/edit_area.php?id=1`, parameter `id` is vulnerable to sql injection:
![image](https://github.com/user-attachments/assets/c7106ba5-045b-4db1-93b9-452eda0781d1)

Apparently, there is no filter for `$_GET['id']`, which leads to sql injection vulnerability.

We can use `sqlmap` to test:
![image](https://github.com/user-attachments/assets/e68402d0-feae-4272-862c-8654d7516d7e)


# PoC
We can use `sqlmap` to test.
Firstly, use burpsuite to intercept traffic packets, copy it in `post.txt` and then
```
sqlmap -r post.txt --dbs --batch
```

![image](https://github.com/user-attachments/assets/e68402d0-feae-4272-862c-8654d7516d7e)
