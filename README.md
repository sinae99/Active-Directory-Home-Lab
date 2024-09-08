# AD-LAB
Setup a basic AD on virtual home network

## Introduction

The main aim of this project is to form a domain and join our guest PC into that domain. after setting up, we create user accounts using a PowerShell script. in the end, we do a bruteforce attack on the domain to check for any obtainable passwords. 

## Lab Set-up and Tools

1 - Windows Server 2019 (AD DS)

2 - Windows 10 (guest)

3 - Ubunto (attacker)

4 - Windows 11 (HOST Machine + SIEM)

## Creating Network

After installing VMs, we will set up Windows Server 2019 to act as our domain controller. To do this, the server needs two network adapters:

External Network (_INTERNET_): This adapter connects to the internet, getting its IP address from the hosts network and the home router.

Internal Network (_INTERNAL_): This adapter allows communication between Windows Server 2019 and the Windows 10 VMs over a private network.


the INTERNET uses the default NAT configuration and INTERNAL will get its IP address manually


## Setting up IP addressing

After identifying the internet and internal adapters (the one with an IP address in your host ip range is the internet adapter), we rename them to INTERNET and INTERNAL.

Now we will be setting up the IP addressing for our INTERNAL adapter :

    IP address: 172.16.0.1
    Subnet mask: 255.255.255.0
    Default gateway: . . .
    Preferred DNS server: 127.0.0.1

lets try "ipconfig" to see what we have done so far
[ipconfig](ipconfig.png)

![ipconfig](https://github.com/user-attachments/assets/4b8e5db7-b488-43f6-8f32-51026559ac53)

## Installing AD

after configuring our machines, the next step is the installation of Active Directory and other roles and features on Windows Server 2019.
we must do these steps :

* Install AD DS
* Promote server to domain controller
* Configure RAS / NAT (to allow VM to access internet through domain)
* Configure DHCP
* Disable the IE Enhanced Security Config setting (to allow server to browse internet from domain controller)

at the end the our server manager is going to be like this :

![AD Installed Roles](https://github.com/user-attachments/assets/cf085b50-b4b3-447e-9093-fbcccaccdd90)


## Creating Users

After creating our domain, the next step is to add users via a PowerShell script and a text file with the usernames. The text file is called [names.txt](names.txt), and the PowerShell script is named [CreateUser.ps](CreateUser.ps). 

The `CreateUser.ps` script functions:

1. **Get Names** : It begins by fetching about 1000 names from the `names.txt` file.
2. **Set Const Password** : All users are assigned the password "Password1".
3. **Create Organizational Unit** : The script creates an Active Directory Organizational Unit (OU) named `_USERS`.
4. **Create and Enable Users** : at the end it uses a foreach loop to create the users in `_USERS` and ensures they are enabled.

this script get a name like Sina Ebrahimi and grab first letter of first name and concat that to family name and use this string as username

Sina Ebrahimi is going to be sebrahimi as username and Password1 as password

![Creating Users](https://github.com/user-attachments/assets/f2ed9d93-c5cb-4317-a4e9-49fd474330b2)





