# Summary
There is a XSS vulnerability in code-project Online Bank (https://code-projects.org/online-bank-in-php-css-javascript-and-mysql-free-download/).

Affected component is `/post_news.php`.

# Details
First, login as **ADMIN**.

In `/post_news.php`,
we post news with this xss payload in `News Headline`:
![image](https://github.com/user-attachments/assets/27274907-9fa7-4484-a9b8-0b39b3e24107)

After submiting, visit `/news.php`:
![image](https://github.com/user-attachments/assets/ae4bd65c-28af-4737-a579-902228c5b7d7)
![image](https://github.com/user-attachments/assets/0ccb035f-7f2c-4745-a80e-1172ec643a0f)


Audit the code, in `post_news_action.php`, there is no XSS filter for `$headline`:
![image](https://github.com/user-attachments/assets/6d8cd1ee-f865-4958-a89d-6410e86717b3)





# PoC
![image](https://github.com/user-attachments/assets/27274907-9fa7-4484-a9b8-0b39b3e24107)
![image](https://github.com/user-attachments/assets/ae4bd65c-28af-4737-a579-902228c5b7d7)
