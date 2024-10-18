# Summary
SourceCodester Best Fee Management System Project has a SQL injection vulnerability in /click_fees/ajax.php?action=save_user, which allows attackers to obtain all data in the website database through sqlmap.

# Details
In url `/click_fees/ajax.php?action=save_user`, 
![image](https://github.com/user-attachments/assets/e1b9025a-bab8-4677-a81f-5ffc9d96002d)

corresponding source code is in `admin_class.php #save_user`:
![image](https://github.com/user-attachments/assets/ad50574b-70bf-47d9-acc9-b5b26aed9dad)

Parameter `username` and `id` are vulnerable to sql injection.

Use burpsuite to intercept the traffic packets:
![image](https://github.com/user-attachments/assets/d783fc44-4188-4f6e-8b37-5dd36776e0a8)

And we can user `sqlmap` to get all of database.
```bash
sqlmap -r post.txt --dbs --batch
```

![sql1_PoC.gif](https://github.com/N0zoM1z0/Vuln-Search/blob/main/sql1_PoC.gif)

# PoC
```bash
sqlmap -r post.txt --dbs --batch
```

![sql1_PoC.gif](https://github.com/N0zoM1z0/Vuln-Search/blob/main/sql1_PoC.gif)
