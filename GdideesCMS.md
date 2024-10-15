# Summary
GDidees CMS **v3.9.1 and lower** versions were found to have an **arbitrary file upload vulnerability**.
After logging in, the attacker can upload any file and gain control of the website.

# Details

![image](https://github.com/user-attachments/assets/edec9c1f-9133-4b3e-a418-7ac0f7bb021c)

![image](https://github.com/user-attachments/assets/d2ce9ce9-179f-46ca-81dd-890567686189)

Although it showed that the file we uploaded was illegal, it was still viewable and not deleted.
![image](https://github.com/user-attachments/assets/15333da1-ef0d-4045-8b60-e9e47c68fbfd)

So, we can access this php file and get a webshell.
![image](https://github.com/user-attachments/assets/02f2dd7f-d0d7-4c01-b19c-8f1641a10cab)


# PoC
![image](https://github.com/user-attachments/assets/02f2dd7f-d0d7-4c01-b19c-8f1641a10cab)
