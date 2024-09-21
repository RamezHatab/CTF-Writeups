# Enumeration
- Critical
- Collecting as much information as possible so we can find vectors of attack
- Point is not to gain access to the target machine, but instead identify all the ways we could attack a target we must find

## Nmap Intro
- Nmap: designed to scan networks and identify available hosts, services and applications using raw packets.
- Ability to identify name, version, OS, etc.

### Use Cases
The tool is one of the most used tools by network administrators and IT security specialists. It is used to:
- Audit the security aspects of networks
- Simulate penetration tests
- Check firewall and IDS settings and configurations
- Types of possible connections
- Network mapping
- Response analysis
- Identify open ports
- Vulnerability assessment as well.

### Syntax
```nmap <scan types> <options> <target>```

Nmap sends one SYN packet, and if it recieves a SYN-ACK back the port is open, but if it gets an RST back it is closed. If nothing, possibly filtered.

## Host Discovery
### Scan Network Range
```sudo nmap 10.129.2.0/24 -sn -oA tnet | grep for | cut -d" " -f5```

![image](https://github.com/user-attachments/assets/78e22e71-6d65-45aa-a6ca-0bb55e02e28c)

### Scan IP List
We can scan a given list of IPs ```hosts.lst``` with:

```sudo nmap -sn -oA tnet -iL hosts.lst | grep for | cut -d" " -f5```

### Activity
Based on the last result, find out which operating system it belongs to. Submit the name of the operating system as result.

Using the fact that ttl of 128 usually indicates a Windows system, that is our answer.



