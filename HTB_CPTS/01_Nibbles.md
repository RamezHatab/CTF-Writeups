# HTB | Nibbles Walkthrough

During this walkthrough I am going to solve the "Nibbles" box on HTB and explain my thought process and how I attack this problem. Let's jump into it! 

## Enumeration
Starting off, the best first step when approaching any machine is always to enumerate. For this box, we were given the target's IP address, OS, and that we are wroking with a web-based vulnerability and attack vector; this means we have a gray-hat scenario since we know some information about the target. However, since we are going to want to uncover deeper information, we need to enumerate.

### Ping
Just to make sure we are connected, let's ping the target machine:

```ping 10.129.200.170```

![image](https://github.com/user-attachments/assets/f0131a47-585e-4ebb-b724-ec15f52e9f36)

Perfect, let's move on!


### Nmap Scan
Let's perform a basic nmap scan to find open ports. We are using ```--open``` to only search for open ports; the ```-sV``` switch to determine the versions of services running on the ports; ```-sC``` to run with default nmap scripts and ```-p-``` to scan all 65,535 ports. We are outputting the data to a file using ```-oA nibbles``` so that we have documentation of the scan for later if needed. Running the command below:

``` nmap -sV -sC -p- --open -oA nibbles 10.129.200.170```

we get this output: 

![image](https://github.com/user-attachments/assets/68370b9a-9d48-4163-9c35-479639b72d6f)

The big takeaways here are...
1. We can confirm the host is likely Ubuntu Linux
2. An OpenSSH version 7.2 server is running on port 22
3. An Apache version 2.4.18 web server is running on port 80

### Grabbing Banners
Quickly, let's use ```nc``` to grab the banners of these two ports and confirm what the nmap scan showed:

![image](https://github.com/user-attachments/assets/ca5ea3d9-d421-4fa6-a860-0a51b4ecd03b)

![image](https://github.com/user-attachments/assets/6b2a9aae-1f4b-4020-8189-bf8870a0ad0a)

From this, we can see that the target is indeed running the servers we anticipated. We didn't get a banner from the scan on port 80, but using verbosity we can see it is indeep connecting to a web server. 

I think we have gotten enough relevant information about the system fromm enumerating to move on to the next step in hacking this box.


## Web Footprinting

### Initial Findings
If we go in blind and navigate to the IP in our browser, we get this webpage:

![image](https://github.com/user-attachments/assets/36ed8577-483f-420f-a88d-cdffc9da18d1)

Since we can't really get anything useful here, the next idea was to investigate the page source code. I found this interesting comment at the bottom:

![image](https://github.com/user-attachments/assets/f1f757b8-24c0-458c-8027-ecbcabc153fd)

We can see there appears to be some sort of nibbleblog directory, so I navigated to ```http://10.129.200.170/nibbleblog/``` and it presents a pretty useless blog page:

![image](https://github.com/user-attachments/assets/6d9bef6a-3895-43dc-a37e-3e098feff2b7)

This leads us to a dead end, so the next step is to find directories to explore.

### Using Gobuster to Enumerate Directories






