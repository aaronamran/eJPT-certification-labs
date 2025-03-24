# Windows Recon: Nmap Host Discovery
Ping the target machine to see if it’s alive or not.
```
ping -c 5 demo.ine.local
```
![image](https://github.com/user-attachments/assets/ade1ff62-96e5-423e-8a79-382f60324990)

We can observe that the target is not responding to the ping requests, so this does not confirm if it’s alive or down. Run a Nmap scan against the target.
```
nmap demo.ine.local
```
![image](https://github.com/user-attachments/assets/8a4bfb3a-c551-421c-b7cb-1035a22ded6c)

Nmap also could not detect whether the host was up or not. Many security tools first ping the host before they start scanning or exploiting the target. In that case, one has to use advanced Nmap options, i.e., -A or -T5, etc., in order to get the correct output.

In the nmap, there is one option, i.e., -Pn (Treat all hosts as online; skip host discovery). This option will force the scanning even if it has detected the target as down in host discovery.

Run Nmap using the -Pn option to discover all alive ports.
```
nmap -Pn demo.ine.local
```
![image](https://github.com/user-attachments/assets/dc9df96a-3a7b-4385-bea7-49e4ebe8f5b4)

We can see multiple ports are open on the target machine.

Now, we will scan any random port that isn’t open. In this case, scan port 443. If the port is not open, we would receive “filtered” output from that port.

```
nmap -Pn -p 443 demo.ine.local
```
![image](https://github.com/user-attachments/assets/8198aa1f-1633-46b9-9513-c4e38dcd404b)
We can observe in the Nmap output that the host is up, but port 443 is filtered.

About Filtered port:

Nmap cannot determine whether the port is open because packet filtering prevents its probes from reaching the port. The filtering could be from a dedicated firewall device, router rules, or host-based firewall software. These ports frustrate attackers because they provide so little information. Sometimes they respond with ICMP error messages such as type 3 code 13 (destination unreachable: communication administratively prohibited), but filters that simply drop probes without responding are far more common. This forces Nmap to retry several times just in case the probe was dropped due to network congestion rather than filtering. This slows down the scan dramatically.

Similarly, if we want to discover the running application on port 80, we could use option -sV, and this option is used to determine the application version information.
```
nmap -Pn -sV -p 80 demo.ine.local
```
![image](https://github.com/user-attachments/assets/a27712d3-e734-4495-aa72-233ffe9e68f9)

<hr />

# Scan the Server 1
Check if the target machine is reachable:
```
ping -c 4 demo.ine.local
```
![image](https://github.com/user-attachments/assets/d73c2cec-88c4-4f8f-9fdc-9e6a1518f419)

The target is reachable. We can now perform a default Nmap port scan on the target to identify the open ports on the target system, this can be done by running the following command:
```
nmap demo.ine.local
```
As shown in the following screenshot, the default Nmap scan does not reveal any open ports. This is because the default Nmap scan profile only scans 1000 of the most commonly used ports.

![image](https://github.com/user-attachments/assets/513e6e41-36ee-4ba0-90de-fe7b1f7594e1)

In order to get an accurate idea of the open ports on the target system, we will need to scan the entire TCP port range (65,535 ports). This can be done by running the following command:
```
nmap demo.ine.local -p-
```

![image](https://github.com/user-attachments/assets/b5191c08-b67a-48e9-a72b-a4f481bcb467)

Now that we have identified the open ports on the target, we can learn more about the services running on the open ports by performing a service detection scan with Nmap.

```
nmap demo.ine.local -p 6421,41288,55413 -sV
```
![image](https://github.com/user-attachments/assets/76bf9f6e-0791-4e4c-85ee-71c9112402f5)

<hr />

# Assessment Methodologies: Footprinting and Scanning CTF1
![image](https://github.com/user-attachments/assets/3052ff77-c87b-4284-8faa-02d92209acaa)

## Q.1 The server proudly announces its identity in every response. Look closely; you might find something unusual.

Run the following command in the terminal:
```
nmap target.ine.local -sC -sV
```
![image](https://github.com/user-attachments/assets/cf418fa5-3aff-4eb0-84dc-e56891d714c8)

Alternatively, you can find this information through the browser’s Network tab.

![image](https://github.com/user-attachments/assets/9d62de74-e02c-41dc-a793-7a4582bbbf95)


## Q.2 The gatekeeper’s instructions often reveal what should remain unseen. Don’t forget to read between the lines.

After performing the Nmap scan, we can see from the results that there are three disallowed entries in the robots.txt file.

![image](https://github.com/user-attachments/assets/0f4d9c3f-7664-46b9-8926-69a94bd50cb5)

Let’s use curl to navigate to /secret-info/, and here we find flag.txt.

![image](https://github.com/user-attachments/assets/ae1a4705-02de-4639-99fa-ade857a6c034)

Next, let’s curl to that file as well:

![image](https://github.com/user-attachments/assets/23d1e073-5384-424a-9cea-b5ac738ff598)


## Q.3 Anonymous access sometimes leads to forgotten treasures. Connect and explore the directory; you might stumble upon something valuable.
During the Nmap scan, we discovered that the FTP server allows anonymous login.
![image](https://github.com/user-attachments/assets/2c7a4ea4-b6c5-4fb5-9f0a-7fb2c6dba8ba)

Let’s connect to the FTP server using the following command: “ftp target.ine.local”. When prompted, enter anonymous as both the username and password for login.

![image](https://github.com/user-attachments/assets/df257e55-8875-4fc8-b29b-044aeb6b4a74)

After successfully logging in, we’ll see two files. We can transfer these files to our system using the get command. Once the files are transferred, exit the FTP server.

![image](https://github.com/user-attachments/assets/8959cdfd-d177-4787-a6e0-9a60efa590f5)

Finally, let’s read the contents of these two files using the cat command.

![image](https://github.com/user-attachments/assets/c9f0c013-69e7-49da-8c79-74810929bfdf)


## Q.4 A well-named database can be quite revealing. Peek at the configurations to discover the hidden treasure.

After transferring both files to our system, the second file, creds.txt, contains the username and password. From the Nmap scan results, we can see that a MySQL server is open on port 3306. Let’s connect to the MySQL server using the provided credentials.

Run the following command:
```
mysql -u db_admin -p -h target.ine.local
```
![image](https://github.com/user-attachments/assets/450c73c7-aee3-41ba-b1c6-9bec5f84a6e1)

Enter the password when prompted. Then, use the command:
```
show databases;
```

This will list all the databases.

![image](https://github.com/user-attachments/assets/f34214df-0daf-4da6-89e6-60ea9844f62b)


Credits to [Prinu_17 on Medium](https://prinugupta.medium.com/assessment-methodologies-footprinting-and-scanning-ctf-1-ejpt-ine-93b02e86bd7a)


<hr />


# Windows Recon: SMB Nmap Scripts
Ping the target machine to see if it’s alive or not.
```
ping -c 5 demo.ine.local
```

![image](https://github.com/user-attachments/assets/53f8ae4e-9cee-4852-90d2-cfc0261bb0ec)


