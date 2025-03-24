# Importing Nmap Scan Results Into MSF

Check if the target machine is reachable:
```
ping -c 4 demo.ine.local
```
![image](https://github.com/user-attachments/assets/61330f28-b5af-4b6a-a297-cb7c76cab2d8)


The target machine is not reachable. Let's use Nmap to skip the host discovery phase and assume that the hosts are online. This is useful in environments where ICMP echo requests (ping) are blocked by firewalls, which would normally prevent Nmap from detecting if a host is up.

Perform the Nmap scan and save the output in XML format
```
nmap -sV -Pn -oX myscan.xml demo.ine.local
```
![image](https://github.com/user-attachments/assets/c7a47498-b7be-450f-a0f7-a6a4e0ed69a9)

After performing an Nmap scan and exporting a scan results XML format, you can import the results directly into the MSF.

To begin with, you will need to start the postgresql database service. This can be done by running the following command:
```
service postgresql start
```
![image](https://github.com/user-attachments/assets/b225fed9-a052-427d-af68-aecc8b5c968a)

Start Metasploit Framework console (msfconsole) and verify the MSF database is connected by running the following command:
```
db_status
```
![image](https://github.com/user-attachments/assets/9bb06150-446c-4b0a-abe9-c9232527d71c)

You can now import the Nmap scan results, by running the following command:
```
db_import myscan.xml
```
![image](https://github.com/user-attachments/assets/3656c9f6-242e-4ef2-9cb3-9b7c5e7cb189)


We can now view the results by running the following commands:
```
hosts
services
```
![image](https://github.com/user-attachments/assets/68e6bd32-1256-465c-9ed1-da748acf30bf)


