# Summary
There is a denial of service vulnerability in `ncc2` binary file of D-Link DIR830LA1_FW100B07.

Fimware download link : https://legacyfiles.us.dlink.com/DIR-830L/REVA/FIRMWARE/DIR-830L_REVA_FIRMWARE_1.00B07.ZIP

Affected component is `Qrs_setWebFilterSettings` and `WebsiteFilter.html`.

Notice: it also exists in **HotFix** version!

# Details
In function `Qrs_setWebFilterSettings`, here get parameter `NumberOfEntry` from user's input:
![image](https://github.com/user-attachments/assets/cb33322f-f5c0-4e04-800c-0ad6071f8f7e)

The following loop obtains the string according to the value of `NumberOfEntry` and assigns the value. There is no upper limit on NumberOfEntry. It should be limited to 15 according to the web page.
![image](https://github.com/user-attachments/assets/e4b49e74-bebf-4bf6-8cc0-69cf8bf93e47)

This results in us being able to change the value of `NumberOfEntry` to a large value after we capture the packet. According to the logic of the program processing, it feels more like an exception caused by out-of-bounds access?
According to my analysis, the a2 pointer is moved back each time, so that the string obtained each time is different:
![image](https://github.com/user-attachments/assets/febea211-7020-4d66-9a6e-9068621005d8)

Funtion `sub_46BF28`:
![image](https://github.com/user-attachments/assets/bba696a4-e38f-4723-8235-46c0e13c4a47)

However, if we only pass a string, except for the first time, no string can be found afterwards, which means that the return value of `strstr` is 0 (NULL).
The program does not judge this situation, that is, whether the return value of strstr != 0, there is:
```
v2 = &v4[v7 + 1];
```

The initial value of v7 is 0, so even if we don’t have a string, v2 will still move back 1 bit each time. If we pass a large number of times (99999999), it will cause an out-of-bounds access.

Front-end interface points:
![image](https://github.com/user-attachments/assets/a9fddb33-fb68-426f-bd69-05bfb85afbac)

We add a rule and use burpsuite to intercept the packet:
![image](https://github.com/user-attachments/assets/a1a158e0-1cda-4028-98a6-0655169a369b)

We change the `NumberOfEntry` to 99999999 and send:
![image](https://github.com/user-attachments/assets/2c9afee7-b63d-4221-b98d-77e1ddce17a8)

It will get stuck for a long time:
![image](https://github.com/user-attachments/assets/d5ad20bc-d3d8-4f1d-9890-363a8cf9c02a)

After a while, we can curl and find that it has 500:
![image](https://github.com/user-attachments/assets/621d1148-48c8-4fb6-b9b8-53766d12cfd4)


# PoC
Visit `/WebsiteFilter.html`:
![image](https://github.com/user-attachments/assets/a9fddb33-fb68-426f-bd69-05bfb85afbac)

We add a rule and use burpsuite to intercept the packet:
![image](https://github.com/user-attachments/assets/a1a158e0-1cda-4028-98a6-0655169a369b)

We change the `NumberOfEntry` to 99999999 and send:
![image](https://github.com/user-attachments/assets/2c9afee7-b63d-4221-b98d-77e1ddce17a8)

It will get stuck for a long time:
![image](https://github.com/user-attachments/assets/d5ad20bc-d3d8-4f1d-9890-363a8cf9c02a)

After a while, we can curl and find that it has 500:
![image](https://github.com/user-attachments/assets/621d1148-48c8-4fb6-b9b8-53766d12cfd4)
