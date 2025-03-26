# Windows Recon: SMB Nmap Scripts
1. Ping the target machine to see if it’s alive or not.
   ```
   ping -c 5 demo.ine.local
   ```
   ![image](https://github.com/user-attachments/assets/53f8ae4e-9cee-4852-90d2-cfc0261bb0ec)
   We can observe that the target machine is alive and we have successfully sent and received all five packets.

2. Run a Nmap scan against the target.
   ```
   nmap demo.ine.local
   ```
   ![image](https://github.com/user-attachments/assets/82bfa8f5-cc97-42a9-a0e2-279e92504156)

3. We have discovered that multiple ports are open. SMB port 445 is also exposed. We will run the Nmap script to list the supported protocols and dialects of an SMB server.
   ```
   nmap -p445 --script smb-protocols demo.ine.local
   ```
   ![image](https://github.com/user-attachments/assets/faa612f7-1907-4a56-ba1c-a1195481355e)

4. Run security mode script to return the information about the SMB security level.
   ```
   nmap -p445 --script smb-security-mode demo.ine.local
   ```
   ![image](https://github.com/user-attachments/assets/cfc08ce8-23c6-4939-b2b5-741c4c7069de)

5. We have tried to access the target SMB server using a guest user and we have received SMB security level information. We can find more information from the following link: [https://nmap.org/nsedoc/scripts/smb-security-mode.html](https://nmap.org/nsedoc/scripts/smb-security-mode.html)

6. We have the SMB server credentials i.e administrator:smbserver_771. We will use it with Nmap script to scan the target to discover sensitive information. Enumerating the users logged into a system through an SMB share with Nmap script. First, we won’t use any credentials to see the output.
   ```
   nmap -p445 --script smb-enum-sessions demo.ine.local
   ```
   ![image](https://github.com/user-attachments/assets/32345913-74b9-4655-9593-f61540abab69)

7. We can observe that on the target machine we have discovered that user bob is logged into without any credentials. This is possible because the target machine is running with the guest login enable configuration and it is a misconfiguration. In case guest login is not enabled we can always use valid credentials of the target machine to discover the same information.
   ```
   nmap -p445 --script smb-enum-sessions --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
   ```
   ![image](https://github.com/user-attachments/assets/0060c573-40b0-4289-a1de-8b1aa198fba0)

8. Enumerating all available shares.
   ```
   nmap -p445 --script smb-enum-shares demo.ine.local
   ```
   ![image](https://github.com/user-attachments/assets/67fafb52-d363-4b93-8293-764ca7ef5ccd)
   ![image](https://github.com/user-attachments/assets/ad9fa67b-c259-4e21-baef-80fc434f7848)

9. We can observe, in the output that we have accessed all the shares using guest users and we have received the permission of each folder or drive. Also, we can notice that IPC$ share has read and write permissions. [About IPC$ share](https://docs.microsoft.com/en-us/troubleshoot/windows-server/networking/inter-process-communication-share-null-session)

   "The IPC$ share is also known as a null session connection. By using this session, Windows lets anonymous users perform certain activities, such as enumerating the names of domain accounts and network shares."

10. Scanning all shares using valid credentials to check the permissions.
   ```
   nmap -p445 --script smb-enum-shares --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
   ```
   ![image](https://github.com/user-attachments/assets/9b952d2a-de91-4603-9513-2ff6b9a72739)
   ![image](https://github.com/user-attachments/assets/a9c3b201-9992-4d74-8b29-328fd856e80d)
   ![image](https://github.com/user-attachments/assets/0bbcde34-8fd5-4a87-82e7-9e71062ff188)

11. We can observe that the administrator user has read and write privilege to the entire C$. i.e C:\ <br />
    Enumerate the windows users on a target machine.
    ```
    nmap -p445 --script smb-enum-users --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
    ```
    ![image](https://github.com/user-attachments/assets/ec5f2963-0729-490e-b8bd-de56a0a06653)

12. We can observe that there are three users present on the target machine. i.e Administrator, bob, Guest.
    Get information about the server statistics. It uses port 445 and port 139 to fetch the details.
    ```
    nmap -p445 --script smb-server-stats --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
    ```
    ![image](https://github.com/user-attachments/assets/51a66c13-f1d9-4635-863b-85758b95bcbd)

13. We can notice that we have received failed logins, permission & system errors, and opened files and print jobs.
    Please note: There is a possibility that the above output would be different in your case which is completely okay.
    
    Enumerate available domains on a target machine.
    ```
    nmap -p445 --script smb-enum-domains --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
    ```
    ![image](https://github.com/user-attachments/assets/8f0b6e53-5346-4c6c-83a3-dd41af844990)

14. We have received the information about the built-in domain on the target machine.
    
    Enumerate available user groups on a target machine.
    ```
    nmap -p445 --script smb-enum-groups --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
    ```
    ![image](https://github.com/user-attachments/assets/01c2e314-401a-4d1d-ac64-21fed92245bc)

15. Enumerate services on a target machine.
    ```
    nmap -p445 --script smb-enum-services --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
    ```
    ![image](https://github.com/user-attachments/assets/dc87f55a-2fd2-4176-ab04-958ed741a610)
    ![image](https://github.com/user-attachments/assets/7e8081d6-bf58-4499-8f0b-dac32b57a3be)
    ![image](https://github.com/user-attachments/assets/8f56fe2b-7ff7-4fd5-892f-ffaf1bc93f40)

16. Enumerate all the shared folders and drives then running the ls command (The ls command is used to list files or directories, similarly dir in windows) on all the shared folders.
    ```
    nmap -p445 --script smb-enum-shares,smb-ls --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
    ```
    ![image](https://github.com/user-attachments/assets/e5bd9b85-bf15-4fbd-96dd-b8a01639dcee)
    ![image](https://github.com/user-attachments/assets/6cb915f0-ad1b-438a-b07f-eeec78c0fee7)
    ![image](https://github.com/user-attachments/assets/7d7260ab-e7d0-4e69-b666-dbdbda359778)
    ![image](https://github.com/user-attachments/assets/abb1a8b6-3a50-4ff8-af5a-4520eafb8b53)
    ![image](https://github.com/user-attachments/assets/09cfddf3-4c53-4d6b-95f2-fffb0d88fb25)






