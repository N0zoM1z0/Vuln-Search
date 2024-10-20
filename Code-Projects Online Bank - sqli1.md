# Summary
There is a sql injection vulnerability in code-project Online Bank (https://code-projects.org/online-bank-in-php-css-javascript-and-mysql-free-download/).

Affected component is `/customer_transactions.php`.

# Details
In `/customer_transactions.php`,
we fill the blank and use burpsuite to intercept traffic packets:
```
POST /customer_transactions.php HTTP/1.1
Host: o-bank:86
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:127.0) Gecko/20100101 Firefox/127.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 59
Origin: http://o-bank:86
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Referer: http://o-bank:86/customer_transactions.php
Cookie: PHPSESSID=0i39o43q69cbi32f8i4s421aip
Upgrade-Insecure-Requests: 1
Priority: u=1

search_term=Balance&date_from=2017-10-02&date_to=2017-10-05
```

Parameter `search_term` is vulnerable to sql injection, we can use `sqlmap` to test:
![image](https://github.com/user-attachments/assets/3d46c512-2cd4-426d-aeab-2db334df47ba)



Audit the code, in `customer_transactions.php`, there is no filter for `$search_term`, `$date_from` and `$date_to`:
![image](https://github.com/user-attachments/assets/186b495a-9872-457f-bbc8-e98f44e044fb)

and these parameters are directly spliced into `$sql0`, which leads to sql injection vulnerability.

![image](https://github.com/user-attachments/assets/e91613d5-0f1e-4c97-8605-8676f768c83e)




# PoC
![image](https://github.com/user-attachments/assets/3d46c512-2cd4-426d-aeab-2db334df47ba)
