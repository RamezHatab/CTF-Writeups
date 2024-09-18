# HTB | Knowledge Check Walkthrough

During this walkthrough I am going to solve the Kowledge Check box at the end of the 'Getting Started' module for CPTS on HTB and explain my thought process and how I attack this problem. Let's jump into it! 

## Enumeration

### Ping
I start by pinging the target and we get 4 responses which means we are connected and ready to go!

### Nmap
Start off with a simple nmap scan. Something like: ```nmap -sV --open 10.129.42.249```. Performing this scan yields these results:

![image](https://github.com/user-attachments/assets/ac57b3dd-db24-48ca-adee-68471ddc5944)

From this, we get information that the target machine is likely running Ubuntu and has an OpenSSH and Apache server running on ports 22 and 80 respectively. Let's
go ahead and confirm these results by grabbing some banners with ```nc```.

![image](https://github.com/user-attachments/assets/40592a27-1504-4c48-909c-3d13d0af3f29)

We can't confirm anything about the Apache version, but we can confirm that nmap provided us the correct info on the OpenSSH server.

Now, for full enumeration, let's run an nmap scan across all ports using ```nmap -sV -sC -p- --open 10.129.42.249```:

![image](https://github.com/user-attachments/assets/4e0d2bff-c22a-4421-87a0-aa86b3edc111)

From here we see that Apache is definitely running a web server, and I think it's time for us to explore it!

## Web Exploration
### Initial findings
Traversing to the machine in FireFox, we come across a website that looks incomplete and seems to use something called GetSimple.
The first thing I did was look up GetSimple vulnerabilities, and I found a remote code execution exploit which exists on GetSimple version 3.3.16.
I want to check to see if that's the version this website is running, so I tried using MetaSploit but we don't have a username and password yet, so I am going to use gobuster to see what we can find.

### Gobuster
Running the command ```gobuster dir -u http://10.129.42.249/ -w /usr/share/dirb/wordlists/common.txt``` results in: 

![image](https://github.com/user-attachments/assets/36065ff3-9494-4df2-adad-37d365708917)

I want to explore all the possible routes we can, so I am going to start with /admin. This takes us to a login page, so now we know we need to find
admin login/password.

Moving to /backups, we find some directories which are all empty. Nothing to see here.

Next, /data. Here, the /user directory catches my eye and we get a huge find in the admin.xml file:

![image](https://github.com/user-attachments/assets/636f8d00-11ec-49b2-a2a1-99a2a67a291d)

it would appear we found

Username = admin
Password = d033e22ae348aeb5660fc2140aec35850c4da997
Email = admin@gettingstarted.com

This ended up not working, but I will save it for later in case we need it.
Next, my intuition led me to the notion that since the website wasn't even set up fully yet, we could try basic USR/PSWD combinations.
admin:admin seems to get us in! 

### Further Exploring
Through searching the website, we find the version of GetSimple running is actually 3.3.15. Doing a quick google search, I find that a RCE exploit
exists on this version, and it is through the ```theme-edit.php``` file where you can upload abitrary code. This will be my way in.

## Setting up a Reverse Shell
### Exploiting the Theme
If we traverse to the 'theme' page and click 'edit theme', we get a .php file which we can alter. I want to see if I can actually execute code
through here, so I am going to add a simple <?php system('id'); ?> at the bottom of the script:

![image](https://github.com/user-attachments/assets/0985dcfc-7565-4775-911e-383671bb96eb)

Now, if we navigate back to the home page ```http://10.129.42.249/```, we see our execution works:

![image](https://github.com/user-attachments/assets/d07a800d-d2c8-4f78-bcf2-0cc0e4b2ae56)

Using this, I am going to set up a reverse shell. Replacing the system id php with our shell:

![image](https://github.com/user-attachments/assets/83bd653b-3716-4684-8156-a43cd4a22be2)

we can now set up a listener using ```nc -lvnp 1234``` to gain access.

After traversing back to the home page, we gain access!

![image](https://github.com/user-attachments/assets/adcded0e-925e-4570-890a-5ab395630e64)


## Gaining Information
We want to turn this simple shell into python, so let's run ```python3 -c 'import pty; pty.spawn("/bin/bash")'``` and upgrade the shell.

Following the steps in the image below, we get our first flag: the ```user.txt``` file~

![image](https://github.com/user-attachments/assets/1d335bb1-5b9a-4137-8cbf-da289361634f)

## Escalating Priveleges
###Getting to Root
Now, the objective is to find our way to root.

I found out about a handy tool called GTFOBins, which gives you different ways to get to root and reverse shell. I used one that seemed applicable since I could use ```sudo``` with no password here:

```CMD="/bin/sh"```

```sudo php -r "system('$CMD');"```

which yielded this:

![image](https://github.com/user-attachments/assets/dcd2d63b-c5d3-48f5-9e28-1730d19eb854)

And jsut like that, we got root.txt!







