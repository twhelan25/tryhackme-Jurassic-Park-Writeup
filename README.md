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

![onlineshop](https://github.com/user-attachments/assets/23cb17db-73db-4694-968a-5a87b7345b8a)

Let's visit the other sections of the online shop:

![shop2](https://github.com/user-attachments/assets/b91d7fbc-661b-4f24-b124-808e849af504)

Notice the url, this alone indicates potential vulnerabilities for sql injection attacks:

Parameter in URL: The presence of a parameter in the URL (id=1) suggests that the application is using this value to query a database. This is a common entry point for SQL injection attacks.
Numeric ID: The use of a numeric ID as a parameter is particularly susceptible to SQL injection if not properly sanitized. Attackers often start by manipulating numeric parameters.
PHP file: The .php extension indicates a PHP script, which is commonly used for database-driven web applications. PHP applications, if not properly coded, can be vulnerable to SQL injection.
Simple structure: The simplicity of the URL structure (just an ID parameter) might indicate a straightforward query to the database, which could be vulnerable if not using parameterized queries.

Before we start testing for sql injection attacks let's finish enuerating directories and run a nikto scan on the target. I dug around the directories but didn't find too much that will help. There is a robots.txt dir which had a reference to the pickle rick CTF. Some some other dirs that contain some hints and others that are related to the sql database. 
Let's try to mess with the numeric parameters that we saw for the url of the online shops.
Each section is 1-3, so first I just changed the number until it brings us to 5, which uncovers valueable info. 

![id=5](https://github.com/user-attachments/assets/e3540c14-649f-490a-94af-8ac72618fe33)

I tried the blocked characters like ', and we get that funny message from Dennis, just like when we hijacked the system in the classic movie! At the end of the page, it says "Try sqlmap... I dare you!".

![magicword!](https://github.com/user-attachments/assets/fb883878-cc48-4f0c-a501-b16b69c9a982)

So, let's start running sqlmap against this url.
