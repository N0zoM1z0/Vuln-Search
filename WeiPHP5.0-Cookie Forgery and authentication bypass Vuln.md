# Summary
WeiPHP 5.0 (https://github.com/weiphpdev/weiphp5.0) has a cookie forgery and authentication bypass vulnerabiity, which allows attacker to bypass authentication to login as admin.

# Details
According to https://github.com/N0zoM1z0/Vuln-Search/blob/main/WeiPHP5.0-Arbitrary%20File%20Download.md, we can download `../config/database.php`, and get `data_auth_key`:
![image](https://github.com/user-attachments/assets/9c39fa2f-47f8-4f15-b25f-e12744e2c7eb)

Notice `application\home\controller\User.php#login` which handles login, if we choose `remember` options, it will put **encrypted** user_id into cookie:
![image](https://github.com/user-attachments/assets/c521e9e8-9d52-4a7c-8b2d-7b5b1632a480)

It is easy to find that only `user_id` value in cookie is used to identify and authentication users.

Now we want to know what `user_id` is.

Audit `application\common.php#think_encrypt`:
![image](https://github.com/user-attachments/assets/49d7c2c5-7fe5-4f76-bcb3-5d25e7a579f7)

Corresponding decrypt function:
![image](https://github.com/user-attachments/assets/d954de4d-3fea-4f52-9d4c-2517c584bc36)

We use this `think_decrypt` funtion to decode our logged in cookie:
![image](https://github.com/user-attachments/assets/01c68095-6a77-40b8-92af-9b5ff3afd951)

So the `user_id` of admin is **1**.

Now we can use `think_encrypt` to forge cookies:
![image](https://github.com/user-attachments/assets/ceda4e77-23dd-49de-899c-99ef502472ce)

```
user_id = MDAwMDAwMDAwMIKDn24
```

After we logged out, just put `user_id = MDAwMDAwMDAwMIKDn24` in cookie and then we can operate as admin.

Take update password as an example,
before we add cookie:
![image](https://github.com/user-attachments/assets/e42cb39c-e9ec-4896-a082-18c87f394206)

after we put value `user_id` we calculate in cookie:
![image](https://github.com/user-attachments/assets/9cc19dba-027c-474d-ab30-942ac2ac35b0)

Apparently, we are operating as admin now.

# PoC
Forging cookie:
![image](https://github.com/user-attachments/assets/ceda4e77-23dd-49de-899c-99ef502472ce)

Take update password as an example,
before we add cookie:
![image](https://github.com/user-attachments/assets/e42cb39c-e9ec-4896-a082-18c87f394206)

after we put value `user_id` we calculate in cookie:
![image](https://github.com/user-attachments/assets/9cc19dba-027c-474d-ab30-942ac2ac35b0)
