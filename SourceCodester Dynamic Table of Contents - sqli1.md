# Summary
There is a sql injection vulnerability in SourceCodester Dynamic Table of Contents `/ajax-api.php?action=delete_count`. 

# Details
Although `view_content` has an is_numeric that is not easy to inject, `delete_content` has no filtering:
![image](https://github.com/user-attachments/assets/30b53243-13e3-489f-88f2-bbbf7faba852)

We can use `sqlmap` to get all the data in the database:
![image](https://github.com/user-attachments/assets/4e957212-5220-4aca-a95a-dabb28bd9192)

Complete process:
![d-toc-sqli1](https://github.com/user-attachments/assets/e4426cfb-49e1-4fe9-ab10-f325cd87ffa4)

# PoC
![d-toc-sqli1](https://github.com/user-attachments/assets/e4426cfb-49e1-4fe9-ab10-f325cd87ffa4)
