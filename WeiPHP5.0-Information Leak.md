# Summary
WeiPHP 5.0 (https://github.com/weiphpdev/weiphp5.0) has an information leak vulnerabiity, which allows attacker to get all uploaded pictures unauthorizedly.

# Details
Audit the code, find that `user_pics.html` can be accessed publicly.
![image](https://github.com/user-attachments/assets/3cf3f533-e5ba-4659-91cc-656f4413ea1d)

According to the route rule of thinkphp, we can visit `/public/index.php/home/file/user_pics` to get the url of uploaded files:
![image](https://github.com/user-attachments/assets/2ce84b33-3cb0-4df0-af09-eb4f0626497e)

![image](https://github.com/user-attachments/assets/4ca04814-52a9-4b1b-a38e-e99a8cbb617f)



# PoC
`/public/index.php/home/file/user_pics`

![image](https://github.com/user-attachments/assets/2ce84b33-3cb0-4df0-af09-eb4f0626497e)

![image](https://github.com/user-attachments/assets/4ca04814-52a9-4b1b-a38e-e99a8cbb617f)
