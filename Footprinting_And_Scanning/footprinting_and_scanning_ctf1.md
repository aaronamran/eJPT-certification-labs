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
