# Summary
"SourceCodester Purchase Order Management System v1.0" was found to have an arbitrary file upload vulnerability.
An attacker can upload a specially crafted file to gain access to the website.

# Details
After logging in, try to change profile picture:
![image](https://github.com/user-attachments/assets/b8be0f88-f46a-4a04-ba1e-15c16aa0baf4)

We can easily upload a `php` file,
![image](https://github.com/user-attachments/assets/8a2d7560-d784-4c0d-bf36-541c4a254670)

After uploading, we open image in new tab, finally we get a webshell:
![image](https://github.com/user-attachments/assets/7c612863-e581-4517-8af9-1016ced18d81)


# PoC
![image](https://github.com/user-attachments/assets/0ae201f5-0a7d-4a75-bdd8-1aadf2fc2bcf)
