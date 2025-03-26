# Apache Enumeration

1. Check if the target machine is reachable:
   ```
   ping -c 5 victim-1
   ```
   ![image](https://github.com/user-attachments/assets/db9cb1fb-61e4-44d6-8d7f-a08aa8fe5c37)
   The target is reachable

2. Open and run the Metasploit auxiliary modules against the target one-by-one.
   Module 1: auxiliary/scanner/http/http_version
   ```
   use auxiliary/scanner/http/http_version
   set RHOSTS victim-1
   run
   ```
   ![image](https://github.com/user-attachments/assets/bd0209c0-4ad6-44ad-8fe9-51a77fea10ee)
   <br />
   Module 2: auxiliary/scanner/http/robots_txt
   ```
   use auxiliary/scanner/http/robots_txt
   set RHOSTS victim-1
   run
   ```
   ![image](https://github.com/user-attachments/assets/f2ed3cc4-5a8c-4380-9e37-f050863cdbdb)
   <br />
   Module 3: auxiliary/scanner/http/http_header
   ```
   use auxiliary/scanner/http/http_header
   set RHOSTS victim-1
   set TARGETURI /secure
   run
   ```
   ![image](https://github.com/user-attachments/assets/6a4076ff-51fd-4ffe-ba49-57161a896ea0)
   <br />
   Module 4: auxiliary/scanner/http/brute_dirs
   ```
   use auxiliary/scanner/http/brute_dirs
   set RHOSTS victim-1
   run
   ```
   ![image](https://github.com/user-attachments/assets/6a0f2c1c-1b0a-49b6-a166-e410153daeae)
   <br />
   Module 5: auxiliary/scanner/http/dir_scanner
   ```
   use auxiliary/scanner/http/dir_scanner
   set RHOSTS victim-1
   set DICTIONARY /usr/share/metasploit-framework/data/wordlists/directory.txt
   run
   ```
   ![image](https://github.com/user-attachments/assets/d4773236-f4b1-4cc1-b69f-e0b822df4561)
   <br />
   Module 6: auxiliary/scanner/http/dir_listing
   ```
   use auxiliary/scanner/http/dir_listing
   set RHOSTS victim-1
   set PATH /data
   run
   ```
   ![image](https://github.com/user-attachments/assets/1905a63a-2c62-4173-b9e5-fa5c198132de)
   <br />
   Module 7: auxiliary/scanner/http/files_dir
   ```
   use auxiliary/scanner/http/files_dir
   set RHOSTS victim-1
   set VERBOSE false
   run
   ```
   ![image](https://github.com/user-attachments/assets/e94dca8c-7dec-4a72-ae4a-99606f7568a9)
   ![image](https://github.com/user-attachments/assets/1bde78de-c1a5-478f-bec6-2fc3d751da8a)
   <br />
   Module 8: auxiliary/scanner/http/http_put
   ```
   use auxiliary/scanner/http/http_put
   set RHOSTS victim-1
   set PATH /data
   set FILENAME test.txt
   set FILEDATA "Welcome To AttackDefense"
   run
   ```
   ![image](https://github.com/user-attachments/assets/5495293d-e71e-408b-b2bc-b52c9a85668c)
   We can observe that we have successfully written a file on the target server. If the file is already exists it will overwrite it. Let’s use wget and download the test.txt file and verify it.
   ```
   wget http://victim-1:80/data/test.txt 
   cat test.txt
   ```
   ![image](https://github.com/user-attachments/assets/3e353087-1d67-4026-8a12-1b48ac0d7f1d)
   We can download the text.txt file and we can see it’s content i.e "Welcome To AttackDefense". Now, let’s use DELETE method and delete the text.file
   ```
   use auxiliary/scanner/http/http_put
   set RHOSTS victim-1
   set PATH /data
   set FILENAME test.txt
   set ACTION DELETE
   run
   ```
   ![image](https://github.com/user-attachments/assets/e528b638-49a1-48f0-b8a3-82d25c003554)
   Let’s try to download the same file from the same path. This time we should receive 404 error. i.e file not found. Because we have deleted it.
   ```
   wget http://victim-1:80/data/test.txt
   ```
   ![image](https://github.com/user-attachments/assets/3459282a-4572-42c7-944c-50ebaeb11296)
   <br />
   Module 9: auxiliary/scanner/http/http_login
   ```
   use auxiliary/scanner/http/http_login
   set RHOSTS victim-1
   set AUTH_URI /secure/
   set VERBOSE false
   run
   ```
   ![image](https://github.com/user-attachments/assets/ef4864e3-9ce7-48b8-81e3-ac440b4d05c4)
   <br />
   Module 10: auxiliary/scanner/http/apache_userdir_enum
   ```
   use auxiliary/scanner/http/apache_userdir_enum
   set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
   set RHOSTS victim-1
   set VERBOSE false
   run
   ```
   ![image](https://github.com/user-attachments/assets/90e453df-87cf-4918-8e4f-d04b213982b9)

   

   




   


   
