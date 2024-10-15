# Summary
`SourceCodester Purchase Order Management System v1.0` was found to have an **arbitrary file upload vulnerability**.
An attacker can upload a specially crafted file to gain access to the website.

# Analyze
You can download source file at this link:"https://www.sourcecodester.com/sites/default/files/download/oretnom23/purchase_order.zip".

We enter this web page interface, choose to modify the avatar, and burpsuite captures the package when uploading:
![image](https://github.com/user-attachments/assets/9db6cbdd-25e7-4909-9866-490bea247a62)
![image](https://github.com/user-attachments/assets/26dc20b5-97ed-4c09-93f4-684c0d803c67)

According to the content of the captured package, find the code here:
`classes\Users.php#save_users` Line33:
![image](https://github.com/user-attachments/assets/bda987a0-9b8b-4205-99cf-b49311f8680a)

It is obvious here that there is an unfiltered arbitrary file upload vulnerability.
In fact, no matter what the conditions are, as long as save_file has an arbitrary file upload vulnerability:
![image](https://github.com/user-attachments/assets/2f4f81b8-8076-4013-af9b-1ec9970d256c)
![image](https://github.com/user-attachments/assets/bb1bccdc-7fe8-4a31-9b45-7c202ee99428)

Then according to the parameter transfer logic of `?f=save`, we will enter this branch:
![image](https://github.com/user-attachments/assets/2bf06f5f-ab9a-44c1-9b9b-b0f4a02e8d45)

Then we will see that the file has been uploaded successfully:
![image](https://github.com/user-attachments/assets/9886da75-5e39-4ec5-99b6-c6c3747cdd86)

![image](https://github.com/user-attachments/assets/481efa37-b0e8-4ed2-a37f-cf1efddc8fd9)


# Details
After logging in, try to change profile picture:
![image](https://github.com/user-attachments/assets/b8be0f88-f46a-4a04-ba1e-15c16aa0baf4)

We can easily upload a `php` file,
![image](https://github.com/user-attachments/assets/8a2d7560-d784-4c0d-bf36-541c4a254670)

After uploading, we open image in new tab, finally we get a webshell:
![image](https://github.com/user-attachments/assets/7c612863-e581-4517-8af9-1016ced18d81)


# PoC
![image](https://github.com/user-attachments/assets/0ae201f5-0a7d-4a75-bdd8-1aadf2fc2bcf)
