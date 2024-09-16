# Summary
SQL injection in the `search` parameter of `SEMCMS_Images.php`.

# Details
The vulnerability trigger point is here:
![image](https://github.com/user-attachments/assets/52db2cf3-d951-46b3-bb0c-5d57f0a84be6)

Audit code, found here:
In `SEMCMS_Images.php` line 181
```php
 <?php
                //
                if ($CatID !== '' && $search !== '') {
                    $sql = $db_conn->query("select * from sc_images where images_category like '%,$CatID,%' and images_name like '%$search%'");
                } else if ($CatID !== '' && $search === '') {
                    $sql = $db_conn->query("select * from sc_images where images_category like '%,$CatID,%'");
                } else if ($CatID === '' && $search !== '') {
                    $sql = $db_conn->query("select * from sc_images where images_name like '%$search%'");
                } else {
                    $sql = $db_conn->query("select * from sc_images");
                }
```
The sql query statement did not filter the **`search`** parameters and directly concatenated them, resulting in a sql injection vulnerability.

# PoC
I use `sqlmap` for test.
![image](https://github.com/user-attachments/assets/bd626aba-fe24-4867-806c-a42b9df21f67)

And then I use
```bash
sqlmap -r post.txt --dbs --batch
```

The result:
![image](https://github.com/user-attachments/assets/0813ed9c-50eb-4229-9799-48ff10cdddc7)

