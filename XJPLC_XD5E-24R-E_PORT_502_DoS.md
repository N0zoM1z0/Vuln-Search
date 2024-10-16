# Summary
Xinjie PLC `XD5E-24R-E` has a denial of service vulnerability. An attacker can send a large number of data packets to cause the PLC to crash.

# Details
Connectivity test of the device:![image](https://github.com/user-attachments/assets/170d5807-3800-4057-a952-4500746c0b37)

Nmap scans device ports:![image](https://github.com/user-attachments/assets/c1f567c7-ee5a-451d-88a5-d82f0f26b3eb)

You can see that port 502 is open.

Then we run the script:
![image](https://github.com/user-attachments/assets/c70b79a4-5655-4411-b41d-166ba959521c)


Soon you will see the device crash. 

And I upload this pcapng file: https://github.com/N0zoM1z0/Vuln-Search/blob/main/XD5E-24R-E-Crashed.pcapng

![image](https://github.com/user-attachments/assets/d899597b-4214-4960-bd99-52ba42a3c858)

![image](https://github.com/user-attachments/assets/65c07cbf-f527-42de-b5d2-27f55d1113e6)

![image](https://github.com/user-attachments/assets/19f967a9-b8c5-4518-844c-4835e9ceccbf)


# PoC
Python script from here : https://github.com/N0zoM1z0/MY-FUZZ .
Just change the `CONFIG.py` to reproduce:
![image](https://github.com/user-attachments/assets/a151f710-dc7f-494e-b45f-d78cbd3568e1)

And then
```
python3 fuzz.py
```

# Discoverer
ylqncepu@163.com

https://github.com/N0zoM1z0
