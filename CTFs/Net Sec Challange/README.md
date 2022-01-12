
# Net Sec Challenge

This is my walk through of the [Net Sec Challenge](https://tryhackme.com/room/netsecchallenge) CTF room on tryhackme.com



## Challenge Questions

Target machine was located at `10.10.245.134`

### What is the highest port number being open less than 10,000? 

Start an Nmap scan: `nmap -T4 10.10.245.134`

![Open Port](./SCreenshots/1-1.png?raw=true 'Open Port')

### There is an open port outside the common 1000 ports; it is above 10,000. What is it?

Scan all ports: `nmap -T4 -p- 10.10.245.134`

![Highest Port](./Screenshots/1-2.png?raw=true 'Highest Port')

### How many TCP ports are open?

Use the same nmap scan as last time.

### What is the flag hidden in the HTTP server header?

Nmap scan: `nmap -T4 -A 10.10.245.134`

![Header Flags](./Screenshots/1-4.png?raw=true 'Header Flags')

### What is the flag hidden in the SSH server header?

Same nmap scan as the previous.

### We have an FTP server listening on a nonstandard port. What is the version of the FTP server?

Since we know the port number from our previous scans, we can just scan that port.

Nmap scan: `nmap -T4 -p 10021 -A 10.10.245.134`

![FTP Version](./Screenshots/1-6.png?raw=true 'FTP Version')

### We learned two usernames using social engineering: eddie and quinn. What is the flag hidden in one of these two account files and accessible via FTP?

I used hydra to crack the ftp passwords

`hydra -l eddie -P /usr/shar/wordlists/rockyou.txt ftp://10.10.245.134:10021` and `hydra -l quinn -P /usr/shar/wordlists/rockyou.txt ftp://10.10.245.134:10021`

FTP into the target machine with the usernames and passwords and look for the flage.

![Flag](./Screenshots/1-8.png?raw=true 'Flag')

### Browsing to `http://MACHINE_IP:8080` displays a small challenge that will give you a flag once you solve it. What is the flag?

Looking at `man nmap`, we can see that we can use the `-sn` flage to avoid detection.

![Challenge](./Screenshots/1-9.png?raw=true 'Challenge')