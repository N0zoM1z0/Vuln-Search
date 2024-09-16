# Summary
**SQL injection** in the `search` parameter of `SEMCMS_Products.php`.

# Details
Vulnerability trigger point:
![image](https://github.com/user-attachments/assets/ef942421-eb0d-4ee4-96da-9e22f48cce07)

Audit code and find here:
In `SEMCMS_Products.php` line 133
```php
if ($CatID!="" && $Searchp!=""){

     $flID=prolmid($CatID,$db_conn);
     $sql=$db_conn->query("select * from sc_products where languageID=".$_GET["lgid"]." and products_name like '%".$Searchp."%' and $flID"); 

   }elseif($CatID!="" && $Searchp==""){

     $flID=prolmid($CatID,$db_conn);
     $sql=$db_conn->query("select * from sc_products where languageID=".$_GET["lgid"]." and $flID");     

   }elseif($CatID=="" && $Searchp!=""){
     
     $sql=$db_conn->query("select * from sc_products where languageID=".$_GET["lgid"]." and products_name like '%".$Searchp."%'");     

   }elseif($indextjs==1){

     $sql=$db_conn->query("select * from sc_products where languageID=".$_GET["lgid"]." and products_index=1 ");   

   }else{

    $sql=$db_conn->query("select * from sc_products where languageID=".$_GET["lgid"]."");  

   }
```
Many parameters, including the "search" parameter, are directly spliced ​​into the SQL query statement without being filtered, resulting in a SQL injection vulnerability.

# PoC
I use `sqlmap` for test:
![image](https://github.com/user-attachments/assets/3bcd34c5-0f55-4aa3-905b-948bebee0e1a)

![image](https://github.com/user-attachments/assets/36d8dd18-636d-4aab-90d1-41fd03e8df9a)

![image](https://github.com/user-attachments/assets/c75912bd-1199-42ba-abe2-5fbefee4e067)
