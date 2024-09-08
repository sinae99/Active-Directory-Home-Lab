# AD-LAB
Setup a basic AD on virtual home network

## Introduction

The main aim of this project is to form a domain and join our guest PC into that domain. after setting up, we create user accounts using a PowerShell script. in the end, we do a bruteforce attack on the domain to check for any obtainable passwords. 

## Lab Set-up and Tools

1 - Windows Server 2019 (AD DS)

2 - Windows 10 (guest)

3 - Ubunto (attacker)

4 - Windows 11 (HOST Machine + SIEM)


## Creating Users

After creating our domain, the next step is to add users. I used a PowerShell script and a text file with the usernames. The text file is called [names.txt](names.txt), and the PowerShell script is named [CreateUser.ps](CreateUser.ps). 
