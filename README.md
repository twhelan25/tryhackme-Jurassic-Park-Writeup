![intro](https://github.com/user-attachments/assets/df530c8e-e71d-4932-87db-b56b28bdad0e)

# tryhackme-Jurassic-Park-Writeup

This is a walkthrough for the tryhackme CTF Jurassic Park. I will not provide any flags or passwords as this is intended to be used as a guide. Think you're up for the Challange?

![Dennis](https://github.com/user-attachments/assets/92daa2f0-353a-490d-8ff2-2f2bf432d062)

## Scanning/Reconnaissance

First off, let's store the target IP as a variable for easy access.

Command: export ip=xx.xx.xx.xx

Next, let's run an nmap scan on the target IP:
```bash
nmap -A -v $ip -D RND:10 -oN nmap.txt
```

Command break down:

-A: This flag enables aggressive scanning. It combines various scan types (like OS detection, version detection, script scanning, and traceroute) into a single scan.

-v: increases verbosity, providing more detailed output during the scan.

—$ip: provides the target IP we stored as the variable $ip.

-D RND:10 Nmap can send additional packets to confuse network intrusion detection systems (IDS) or hide the true source of the scan by randomly selecting up to 10 decoy IP addresses.

-oN nmap.txt: This option specifies normal output that should be saved to a file named “nmap.txt.

This scan reveals a webserver on port 80 and ssh on tcp/22:

![nmap](https://github.com/user-attachments/assets/1d1a853a-bce9-46c4-8fe9-06478e544fbe)

Let's check out the webserver on tcp/80 first:

