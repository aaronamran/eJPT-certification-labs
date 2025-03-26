# Samba Recon: Basics

1. Find the default tcp ports used by smbd
   ```
   nmap demo.ine.local
   ```
   ![image](https://github.com/user-attachments/assets/b26c78a7-acc6-4fa5-85f6-c0b1f345288a)
   139, 445

2. Find the default udp ports used by nmbd
   ```
   nmap -sU --top-ports 25 demo.ine.local
   ```
   ![image](https://github.com/user-attachments/assets/69d5cdc6-0d47-4b60-a374-a5a9a5a84163)
   137, 138
   
3. What is the workgroup name of samba server?
   ```
   nmap -sV -p 445 demo.ine.local
   ```
   ![image](https://github.com/user-attachments/assets/3a8d1fbe-3fb7-4f6d-8112-dff0f5aeb9be)
   RECONLABS

4. Find the exact version of samba server by using appropriate nmap script.
   ```
   nmap --script smb-os-discovery.nse -p 445 demo.ine.local
   ```
   ![image](https://github.com/user-attachments/assets/2fb2a46e-ecdd-42bd-8493-98e0f6c2f56c)
   Samba 4.3.11-Ubuntu
   
5. Find the exact version of samba server by using smb_version metasploit module.
   ```
   msfconsole -q
   use auxiliary/scanner/smb/smb_version
   set RHOSTS demo.ine.local
   exploit
   ```
   ![image](https://github.com/user-attachments/assets/95013c44-1daa-4f9a-930c-2d04f45e563a)
   Samba 4.3.11-Ubuntu

6. What is the NetBIOS computer name of samba server? Use appropriate nmap scripts.
   ```
   nmap --script smb-os-discovery.nse -p 445 demo.ine.local
   ```
   ![image](https://github.com/user-attachments/assets/6d926790-38da-43ab-928e-3e6bc39822f0)
   SAMBA-RECON

7. Find the NetBIOS computer name of samba server using nmblookup.
   ```
   nmblookup -A demo.ine.local
   ```
   ![image](https://github.com/user-attachments/assets/2d04817c-448e-4b8f-abe7-04e24b026bc1)
   SAMBA-RECON

8. Using smbclient determine whether anonymous connection (null session) is allowed on the samba server or not.
   ```
   smbclient -L demo.ine.local -N
   ```
   Anonymous connection is allowed since shares are displayed without requirement of password.
   ![image](https://github.com/user-attachments/assets/97c22000-1ebd-43f7-a23c-eda286901a18)
   Allowed

9. Using rpcclient determine whether anonymous connection (null session) is allowed on the samba server or not.
   ```
   rpcclient -U "" -N demo.ine.local
   ```
   Anonymous connection is allowed since no errors are thrown while connecting to samba serverwithout any credentials.
   ![image](https://github.com/user-attachments/assets/aa3162ed-bdaa-40d2-92ee-76a2bd69caed)
   Allowed


   



   
   

   

   


