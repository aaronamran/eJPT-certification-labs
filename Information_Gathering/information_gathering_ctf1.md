# Assessment Methodologies: Information Gathering CTF 1

![image](https://github.com/user-attachments/assets/d346572f-cde0-49ce-9782-0b819e255a4b)

## Q.1 This tells search engines what to and what not to avoid.
The robots.txt file tells search engines what to crawl and what to avoid. Let’s take a look at the robots.txt file, and here we find our first flag.

![image](https://github.com/user-attachments/assets/9c711f53-7d0f-452a-af26-77901e376390)

## Q.2 What website is running on the target, and what is its version?
Run the following command in the terminal:
```
nmap target.ine.local -sC -sV
```

![image](https://github.com/user-attachments/assets/87951531-c324-49b2-bb51-0702fc50b085)


## Q.3 Directory browsing might reveal where files are stored.
We need to brute-force the directories, and we can use the simple dirb command. Once we run the scan, we will need to manually search for the flag.

![image](https://github.com/user-attachments/assets/6077e10a-837b-4528-bec1-f20346639782)
<br />
![image](https://github.com/user-attachments/assets/173e11f5-a741-4e04-aeb9-4e3eb3a5dcee)
The flag is located in the wp-content/uploads directory.
![image](https://github.com/user-attachments/assets/b3c0f0d5-50ad-40bc-847f-b85e8a92203b)

## Q.4 An overlooked backup file in the webroot can be problematic if it reveals sensitive configuration details.
To find the backup files, we need to use the -X option in the command to specify the file extensions. The most common backup file extensions are: .bak, .tar.gz, .zip, .sql, and .bak.zip.
Run the following command in Terminal: 
```
dirb http://target.ine.local -w /usr/share/dirb/wordlists/big.txt -X .bak,.tar.gz,.zip,.sql,.bak.zip
```
![image](https://github.com/user-attachments/assets/08ca5f97-c81f-4ea1-b9ad-01433ebd612f)

We can use the curl command to read its contents. And here, we find our fourth flag, which is:
```
curl http://target.ine.local/wp-config.bak
```
![image](https://github.com/user-attachments/assets/807eece6-09fe-46fc-9a7c-debc748c58f9)


## Q.5 Certain files may reveal something interesting when mirrored.
As the question suggests, we need to mirror the website to find this flag. To mirror the website, we can use the httrack command:
```
httrack http://target.ine.local -O target.html
```

![image](https://github.com/user-attachments/assets/545e6d36-2d93-4d8b-9d77-34712774a698)

Once the mirroring is complete, navigate to the directory where the website was saved. The flag is located in the file xmlrpc0db0.php.

![image](https://github.com/user-attachments/assets/12e7cba6-20af-4f51-9039-30c1ae48848a)


Credits to [Prinu_17 on Medium](https://prinugupta.medium.com/assessment-methodologies-information-gathering-ctf-1-ejpt-ine-f9498ba88d61)
