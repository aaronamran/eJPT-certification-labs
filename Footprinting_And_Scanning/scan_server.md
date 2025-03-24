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
