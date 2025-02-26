# AD-LAB
Setup a basic AD on virtual home network

## Introduction

The main goal of this project is to set up a domain and connect a guest PC to it. After configuring the network and DC, we will create user accounts. Then, we will join the client to the domain and verify that everything is working correctly.




## Lab Set-up and Tools

1 - Windows Server 2019 (AD DS)

2 - Windows 10 (guest)

3 - Windows 11 (HOST Machine)

This is the diagram of what I want to build.


![Diagram](https://github.com/user-attachments/assets/c9e8fa13-c4bd-4178-8700-e0b3af6cab08)



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

trying "ipconfig" to see what we have done so far

![ipconfig](https://github.com/user-attachments/assets/4b8e5db7-b488-43f6-8f32-51026559ac53)

## Installing AD

after configuring our machines, the next step is the installation of Active Directory and other roles and features on Windows Server 2019.
we must do these steps :

* Install AD DS
* Promote server to domain controller
* Configure RAS / NAT (to allow VM to access internet through domain)
* Configure DHCP
* Disable the IE Enhanced Security Config setting (to allow server to browse internet from domain controller)

  
after all installations 
at the end our server manager is going to be like this :

![AD Installed Roles](https://github.com/user-attachments/assets/cf085b50-b4b3-447e-9093-fbcccaccdd90)


## Creating Users

After creating our domain, the next step is to add users via a PowerShell script and a text file with the usernames. The text file is called [names.txt](names.txt), and the PowerShell script is named [CreateUser.ps](CreateUser.ps). 

The `CreateUser.ps` script explanation :

1. **Get Names** : It begins by getting about 1000 names from the `names.txt` file.
2. **Set Const Password** : All users are assigned the password "Password1".
3. **Create Organizational Unit** : The script creates an AD Organizational Unit named `_USERS`.
4. **Create and Enable Users** : at the end it uses a foreach loop to create the users in `_USERS` and ensures they are enabled.

this script get a name like Sina Ebrahimi and grab first letter of first name and concat that to family name and use this string as username

Sina Ebrahimi is going to be sebrahimi as username and Password1 as password

![Creating Users](https://github.com/user-attachments/assets/f2ed9d93-c5cb-4317-a4e9-49fd474330b2)

checking our created domain (firstdomain) and some users :

![firstdomain](https://github.com/user-attachments/assets/b5b93de4-1e05-4d5c-aa2e-1fe3325eabc5)

DC setup and user creation is Done.

## Install Windows 10 Client

After creating the domain, it’s time to install the Windows 10 client.

I am going to install Windows 10 Enterprise for this (somehow, Home Edition wont work! idk why), and afterward, we need to add this PC to the domain.

Setting > About > Rename this PC > Advanced and then change domain or workgroup
in Computer Name/Domain Changes select member of Domain and type "firstdomain"
im going to use acoke as user

and after all this is the final result :

![welcome to firstdomain](https://github.com/user-attachments/assets/ac7c79dd-39bb-4f81-a7f1-b58137dc6443)

## Final Check

![check joined user](https://github.com/user-attachments/assets/e4341eff-dce4-4293-b634-2160830a5c5f)

everything is fine now.

and for the last step lets check DHCP and Active Directory Users and Computers from our Server manager :


![AD Users and Computers check](https://github.com/user-attachments/assets/8c1bb46e-8053-4ffa-8425-cf4e79f26453)


![DHCP check](https://github.com/user-attachments/assets/cb1a5c70-5e04-429f-b079-346da542f85e)


as above our windows 10 (Client10) is added to Leased Addresses and Computers.

Lab Set up is Done.







