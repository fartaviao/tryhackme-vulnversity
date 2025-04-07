# TryHackMe - Vulnversity Machine Documentation

## Introduction
This guide provides a detailed step-by-step walkthrough for the **Vulnversity** machine on TryHackMe. It covers network connectivity, reconnaissance, exploitation, and privilege escalation to obtain the final flag.

## Prerequisites
Before starting, ensure you have:
- An account on [TryHackMe](https://tryhackme.com/)
- A machine with **Kali Linux or Parrot Security** (VM in **VirtualBox** or **VMware** as well)
- A stable internet connection
- OpenVPN installed to connect to TryHackMe VPN

### Required Tools:
`nmap`, `gobuster`, `burpsuite`, `netcat`, `python`, `find`

---
## Repository Structure

```
tryhackme-vulnversity/
├── README.md                      # Introduction and overview
├── Vulnversity.md                 # Main documentation (full guide)
├── Scripts/
│   └── reverse-shell.phtml        # Reverse shell script
└── Screenshots/                   # Visual references
    ├── Screenshot-01.png
    ├── Screenshot-02.png
    ├── ...
    ├── Screenshot-37.png
    └── Screenshots.md
```
---
## Task 1: Deploy and Connect to TryHackMe VPN

1. **The first step is to establish a secure connection with the TryHackMe platform using OpenVPN.**
   - You can follow this guide [VPN Secure Guide](https://github.com/fartaviao/tryhackme-tutorial) for detailed documentation and step-by-step process connect your machine to TryHackMe VPN.
   - In this case we are doing the [Vulnversity](https://tryhackme.com/room/vulnversity) machine, therefore make sure to join the room and start the machine to see the target IP. 
   - This is the first step to gain access to the target machine, ensure you have completed before continue.
   
2. **For be more organized we can follow the following structure ~/Downloads/TryHackMe/Vulnversity**
 - In my case I will continue using the Downloads folder.
   ```bash
   cd Downloads/TryHackMe
   mkdir Vulnversity
   ls
   cd Vulnversity
   ```
- No answer needed ✅ Correct Answer
---
## Task 2: Reconnaissance with Nmap

   - Gather information on this machine using a network scanning tool called Nmap.
   - Nmap is a free, open-source, and powerful tool used to discover hosts and services in a computer network.
   - We can use Nmap to scan a machine and identify all the services running on a specific port.
   - Nmap has many capabilities; below is a table summarizing some of its functionalities.

| Nmap Flag | Description |
|-----------|------------|
| `-sV` | Attempts to determine the version of running services |
| `-p <x>` or `-p-` | Scans the specified port `<x>` or all ports |
| `-Pn` | Disables host discovery and scans open ports |
| `-A` | Enables OS and version detection, runs internal scripts for additional enumeration |
| `-sC` | Scans using Nmap’s default scripts |
| `-v` | Verbose mode (detailed output) |
| `-sU` | UDP port scanning |
| `-sS` | TCP SYN port scanning |

   - Nmap is an essential tool for cybersecurity professionals and network administrators, allowing them to analyze network security and detect vulnerabilities.


1. **Scan for open ports and services:**
```bash
   nmap -sV <MACHINE_TARGET_IP>
```
![Scan for open ports and services](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-01.PNG)

2. **Identified open ports:**
   - **21/tcp** - FTP (vsftpd 3.0.3)
   - **22/tcp** - SSH (OpenSSH 7.2p2 Ubuntu)
   - **139, 445/tcp** - Samba (SMB service)
   - **3128/tcp** - Squid proxy (3.5.12)
   - **3333/tcp** - Apache HTTP Server (2.4.18 Ubuntu)
3. **Key Information:**
   - **Operating system:** `Ubuntu`
   - **Web server port:** `3333`
   - **Verbose mode flag in Nmap:** `-v`
   
   **Nmap Scanning Questions and Answers**

 ## Answer the questions below:

**1. There are many Nmap "cheatsheets" online that you can use too.**
- No answer needed ✅ Correct Answer

**2. Scan the box; how many ports are open?**
- 6 ✅ Correct Answer

**3. What version of the Squid proxy is running on the machine?**
- 3.5.12 ✅ Correct Answer

**4. How many ports will Nmap scan if the flag `-p-400` was used?**
- 400 ✅ Correct Answer

**5. What is the most likely operating system this machine is running?**
- Ubuntu ✅ Correct Answer

**6. What port is the web server running on?**
- 3333 ✅ Correct Answer

![Web server site](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-02.PNG)

**7. It's essential to ensure you are always doing your reconnaissance thoroughly before progressing. Knowing all open services (which can all be points of exploitation) is very important. Don't forget that ports in a higher range might be open, so constantly scan ports after 1000 (even if you leave checking in the background).**
- No answer needed ✅ Correct Answer

**8. What is the flag for enabling verbose mode using Nmap?**
- `-v` ✅ Correct Answer

---
## Task 3: Locating directories using Gobuster

Gobuster is a tool for brute-forcing URIs (directories and files), DNS subdomains, and virtual host names.
For this machine, we will focus on using it to brute-force directories.

- Installing Gobuster

To install Gobuster, run the following command:

```bash
sudo apt-get install gobuster
```

- Wordlists for Gobuster

Gobuster requires a wordlist to identify publicly available directories. If you're using Kali Linux or Parrot OS, you can find wordlists under:

```
/usr/share/wordlists
```

For directory brute-forcing, you can use:

```
/usr/share/wordlists/dirbuster/directory-list-1.0.txt
```

- Running Gobuster

To scan a website for hidden directories, use the following command:

```bash
gobuster dir -u http://10.10.31.200:3333 -w /usr/share/wordlists/dirbuster/directory-list-1.0.txt
```
- Command Breakdown

- `gobuster dir`: Tells **GoBuster** to perform a directory scan.
- `-u http://10.10.31.200:3333`: Specifies the target URL to scan. In this case, the scan is performed on the web server at IP **10.10.31.200**, port **3333**.
- `-w /usr/share/wordlists/dirbuster/directory-list-1.0.txt`: Indicates the path to the **wordlist** used for the scan. Here, it uses the list located at `/usr/share/wordlists/dirbuster/directory-list-1.0.txt`.

> 📌 **Note**: Make sure the wordlist file exists at the specified path. You can use different wordlists depending on the challenge or environment.

### Gobuster Flags

| Gobuster flag   | Description |
|----------------|-------------|
| `-e`          | Print the full URLs in your console |
| `-u`          | The target URL |
| `-w`          | Path to your wordlist |
| `-U` and `-P` | Username and Password for Basic Auth |
| `-p <x>`      | Proxy to use for requests |
| `-c <http cookies>` | Specify a cookie for simulating your auth |

This tool is particularly useful for penetration testing and security assessments, helping to uncover hidden directories and resources on a target system.

- Answer the questions below:

1. **I have successfully configured Gobuster.**
- No answer needed ✅ Correct Answer

2. **What is the directory that has an upload form page?:**
- `/internal/` ✅ Correct Answer

![Gobuster search](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-03.PNG)

- You can enter the directories pulse Ctrl + clic

![Web server site upload form](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-04.PNG)

- We can check that this directory has an upload form


---
## Task 4: Compromise the Webserver

Now that you have found a form to upload files, we can leverage this to upload and execute our payload, which will lead to compromising the web server.

- Answer the questions below:

1. **What common file type you'd want to upload to exploit the server is blocked? Try a couple to find out.**
- Logically, the ideal would be to upload a .php file to execute code, like a reverse shell. A "reverse shell" is a technique that involves
establishing a connection from the compromised machine (the victim) to the attacker, instead of the attacker connecting to the victim like in a typical
connection. This allows the attacker to gain remote access to the compromised system and execute commands on it.
In a reverse shell scenario, the attacker sets up a program or script on the target machine that initiates a network connection to an IP address and
port controlled by the attacker. Once the connection is established, the attacker can send commands to the compromised system and receive the
responses. Imagine it as a Trojan horse.

- .php ✅ Correct Answer

2. **I understand the Burpsuite tool and its purpose during pentesting.**
- We will fuzz the upload form to identify which extensions are not blocked. To do this, we'll use BurpSuite. We’re going to use Intruder (used to automate custom attacks).
To start, make a wordlist with the following extensions:
```bash
nano phplist.txt
```
```
.php
.php3
.php4
.php5
.phtml
```

```bash
cat phplist.txt
```
Ctrl + x and then y to save the file.

![Make a wordlist](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-05.PNG)

To get started, we’ll begin by installing FoxyProxy in the Firefox browser.
FoxyProxy is a browser extension for Firefox and Chrome that makes it simple to manage and switch between different proxy settings.
It lets you set up specific rules so that certain websites or types of content are accessed through different proxy servers based on things like the 
site's address, domain name or content type.
This tool is especially helpful for people who need to reach resources on different networks or want to stay anonymous while browsing.
FoxyProxy makes handling proxies easier by offering a user-friendly interface within the browser, allowing you to quickly change, activate,
or turn off proxy settings as needed.

- Search "foxyproxy extension" and add it
![Add foxyproxy extension](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-06.PNG)
![Add foxyproxy extension](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-07.PNG)

- We go into the FoxyProxy options and add a new proxy and click Save.

Title: Burp

Type: HTTP

Hostname: 127.0.0.1

Port: 8080

![Foxyproxy Options](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-08.PNG)
![Add a new proxy and click Save](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-09.PNG)

- Now we run Burpsuite from the terminal. (It is preinstalled in Kali or Parrot OS)

```bash
burpsuite
```
- Burpsuite will open, we can create a temporary project, use the default parameters, and launch it by clicking Start Burp.

![Create a temporary project](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-10.PNG)

- Go to the Proxy tab, Intercept, and put the option in "Intercept is ON".

![Intercept is ON](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-11.PNG)

- At this point, BurpSuite is intercepting. We can go back to Firefox and select the burpsuite Proxy we previously configured.

![Select the burpsuite Proxy](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-12.PNG)

- Now we can upload the file we’ve created. Click on Submit, and we notice that the page keeps loading because the proxy has intercepted the request.
So, we go to BurpSuite to see what has been intercepted.

![Upload the file we’ve created](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-13.PNG)

- We obtain the HTML request information in text format. Additionally, we can see the file we’re trying to upload to the target machine,
with the plain text content showing the different extensions included in our file `phplist.txt`

![HTML request information](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-14.PNG)
(The target IP changed because in my case for this write-up the machine timed out, but it doesn't matter just keep it in mind)

- Next step is select and highlight the `phplist.txt`, right-clic and tab in Send to Intruder, then go to the Intruder tab.
- In Burp Suite, the Sniper attack is a type of attack used within the Intruder tool. The Sniper attack is used for brute force or data injection 
attacks on a single HTTP request or individual parameter.
- When setting up a Sniper attack in Burp Suite, you select a single input field in an HTTP request and provide a list of payloads or values to test. 
- Burp Suite then sends the request repeatedly, each time with a different payload, aiming to find a different response or server behavior for each 
tested payload.
- In our case, we are going to carry out an attack by testing different file extensions, to determine which one is accepted and upload the file with the correct extension.
- For this we have to pulse on `clear§`

![Send to Intruder and clear§](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-15.PNG)

- Next, we select and highlight the `.txt` extension, so when we carry out the attack, that extension is replaced by the ones we add to the payload.
- To do this, we select and highlight the extension and click `Add§`. It should look like this:

![Extension replaced](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-16.PNG)

- We go to the Payloads tab which is displayed and load the file we created with the extensions: `phpext.txt`.
- We disable payload encoding. Payload encoding in BurpSuite is a feature that allows you to automatically encode payloads before sending them in an Intruder Sniper attack. This can be useful when you need to bypass input restrictions, such as character filters or security mechanisms that attempt to detect and block certain data patterns. In our case, we disable it.

![Load the payload](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-17.PNG)

- Click on `Start attack`.

![Start the attack](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-18.PNG)

- We get the attack result, the payload has tested each of the extensions, and each one gets a response.

![Attack result](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-19.PNG)

- To check the response, we click on one of the extensions, go to the Response tab, and look at line 34, where we can see whether the extension is allowed or not.

![Check the response](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-20.PNG)

- We must check each of the extensions until we find which is allowed.

![Check each of the extensions](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-21.PNG)

- We’ve found an allowed extension: `.phtml`. PHTML, which means for "PHP Hypertext Preprocessor HTML", is a file extension mainly used in web applications for pages containing embedded PHP code. It is essentially an HTML file with PHP code fragments inside. When a web server receives a request for a file with the .phtml extension, it interprets and executes the PHP code before sending the resulting page to the client.
- Therefore, this is the file extension that we can upload to the target system to create our reverse shell.
- Now we can close BurpSuite, disable FoxyProxy, and continue.
- Following the instructions from TRYHACKME, we are going to execute a Reverse Shell mentioned earlier.
- Now that we know which extension we can use for our payload, we can proceed.
- We are going to use a reverse shell written in PHP as our payload. A reverse shell works by making a connection from the victim machine to the attacker's machine, forcing the target to initiate a connection with you. So, you’ll be listening for incoming connections, receiving, uploading, and executing your shell, which will send you a signal so you can control it.
- Download the following PHP reverse shell from [here](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php)

![Download the PHP reverse shell](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-22.PNG)

- To gain remote access to this machine, follow these steps:

1. Move the file to the `Vulnversity` folder to be more organised:
```
mv ~/Downloads/php-reverse-shell.php ~/Downloads/TryHackMe/Vulnversity
ls
```
2. Rename the file with the extension allowed:
```
mv php-reverse-shell.php php-reverse-shell.phtml
ls
```
3. Edit the php-reverse-shell.phtml file and change the IP address to your tun0 interface IP (you can get it with the `ip a` command).
```
nano php-reverse-shell.phtml
```
Ctrl + x and then y to save the file.

![Edit the php-reverse-shell.phtml file](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-23.PNG)

- We also check the port (in this case, it’s set to 1234, you can change it). This is important when running Netcat, as we’ll specify the port our system is listening on.

![Edit the php-reverse-shell.phtml file](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-24.PNG)

4. Now we can run netcat indicating the port on which we will be listening:
```
nc -lvnp 1234
```
![Run netcat](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-25.PNG)

5. Now with netcat listening the 1234 port we can load the .phtml file with the reverse shell.

![Load the .phtml file with the reverse shell](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-26.PNG)

- With confirmation that the file has been successfully uploaded to the server, the following question arises: Where is the file we just uploaded with the .phtml extension? We need to search in the server for that file. To do this, we use `Gobuster` as we did before, but this time we perform the scan targeting the `/internal/` directory directly:
```
gobuster dir -u http://10.10.41.197:3333/internal -w /usr/share/wordlists/dirbuster/directory-list-1.0.txt
```
- We found a /uploads directory. We can access it with Ctrl+click to see the file we just uploaded.

![Search and execute the reverse shell](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-27.PNG)

- Now we just need to run it by clicking on it. If everything works correctly, the window will appear to be loading, and if we return to the terminal where we had Netcat listening we'll see that the reverse shell has worked correctly. Now let’s start a new instance of the shell using the following command:
```
python -c 'import pty; pty.spawn("/bin/bash")'
```
- `python -c`: This runs the Python interpreter in command execution mode.
- `'import pty; pty.spawn("/bin/bash")'`: This is the Python script being executed. It imports the pty module (which provides functions to handle pseudo-terminals) and then calls the spawn() function from the module, passing "/bin/bash" as the argument. This starts a new process tied to a pseudo-terminal, effectively giving us an interactive /bin/bash shell.

![Start a new instance of the shell](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-28.PNG)

**Now come back to the questions for the Task 4: 2. I understand the Burpsuite tool and its purpose during pentesting.**
- No answer needed ✅ Correct Answer

3. **What extension is allowed after running the above exercise?**
- .phtml ✅ Correct Answer

4. **While completing the above exercise, I have successfully downloaded the PHP reverse shell.**
- No answer needed ✅ Correct Answer

5. **What is the name of the user who manages the webserver?**
- To display the list of users on the machine, we need to cat the /etc/passwd file, which is where information about user accounts is stored. We use the following command:
```
cat /etc/passwd
```
- It will display the list of all user accounts. If we look at the last line, we see a user named `bill` with the `/home` directory:

![User who manages the webserver](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-29.PNG)

- bill ✅ Correct Answer

6. **What is the user flag?**
- We can move into the `/home/bill` directory and cat the user flag:
```
cd /home/bill
ls
cat user.txt
```

![User flag](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-30.PNG)

- 8bd7992fbe8a6ad22a63361004cfcedb ✅ Correct Answer

---

## Task 5: Privilege Escalation
Now that you have compromised this machine, we will escalate our privileges and become the superuser (root).
In Linux, SUID (set owner userId upon execution) is a particular type of file permission given to a file. SUID gives temporary permissions to a user to run the program/file with the permission of the file owner (rather than the user who runs it).
For example, the binary file to change your password has the SUID bit set on it (/usr/bin/passwd). This is because to change your password, you will need to write to the shadowers file that you do not have access to; root does, so it has root privileges to make the right changes.

- Understanding SUID in Linux

The image below demonstrates how the SUID (Set User ID) bit modifies file permissions in Linux:

![Bin SUID](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-31.PNG)

- What is SUID?

The **SUID** bit allows a user to execute a file with the privileges of the file owner (usually root), instead of the privileges of the user who runs the file.

- Permission Breakdown

- `rwx` → read, write, execute
- The numbers above (`421`) are the octal values of each permission:
  - `4` = read (`r`)
  - `2` = write (`w`)
  - `1` = execute (`x`)

- What happens with SUID?

When the SUID is set on the **user** owner:

- The execute (`x`) permission becomes a lowercase or uppercase **s** (`s` or `S`).
- It appears in the user position, like: `rws------`

- If the execute bit is not set and SUID is enabled, it will display as `S` (uppercase), indicating a misconfigured SUID.

- Real Example:

```bash
ls -l /usr/bin/passwd
```

Typical output:

```bash
-rwsr-xr-x 1 root root 54256 Jan 18 2025 /usr/bin/passwd
```

- Meaning:
  - `s` in the user block indicates that the SUID bit is set.
  - The `passwd` file runs with root privileges even if another user executes it.

- Why is it important?

The SUID bit can be a vector for privilege escalation if misconfigured. In pentesting or security auditing, finding files with active SUID bits is essential to detect potential security flaws.

- Run the command to find files with SUID:

```bash
find / -perm -u=s -type f 2>/dev/null
```

This command lists all files with the SUID bit enabled on the system.

---

1. **Now come back to the questions for the Task 5: 1. On the system, search for all SUID files. Which file stands out?**

- /bin/systemctl ✅ Correct Answer

![/bin/systemctl](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-32.PNG)

- Why is `systemctl` with SUID is important and so Dangerous?

During privilege escalation, discovering **`/bin/systemctl`** with the **SUID bit** set is a major finding.

- What is `systemctl`?

`systemctl` is the primary tool for managing **systemd services**. It allows users to:

- Start/stop/restart services
- Enable/disable services at boot
- View system logs and states
- Switch targets (runlevels)
- Reload daemons and configurations

- What Makes `systemctl` with SUID Special?

Having the **SUID bit** on `systemctl` means it **runs with root privileges**, even if executed by a normal user. This opens multiple paths for privilege escalation:

- Critical Risks

Start/Stop Critical Services examples: Disabling firewalls (`ufw`, `iptables`), antivirus (`clamav`), auditing tools, etc.

Create & Launch Malicious Services: An attacker can craft a malicious `.service` file to execute a shell.

Total Privilege Escalation
The attacker can:
- Run arbitrary commands as root
- Modify system configs
- Establish persistence
- Even reboot/shutdown the machine

- How to Check if `systemctl` is SUID?

```bash
ls -l /bin/systemctl
```

If you see:

```bash
-rwsr-xr-x 1 root root 152600 Jan 1 12:34 /bin/systemctl
```

The `s` in `rws` means it has **SUID**, which is dangerous.

---

2. **What is the root flag value?**

Taking advantage of the fact that the systemctl binary has the SUID bit enabled, let’s take a look at [GTFOBins](https://gtfobins.github.io/) and search for systemctl.

![GTFOBins systemctl SUID](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-33.PNG)

- We can search for `systemctl`, access SUID and copy the code that we are going to use to escalate privileges.

![GTFOBins systemctl SUID](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-34.PNG)

- Open a new terminal, paste the code into a text editor and make the following changes:
```
nano systemctlsuid.txt
```

- It should look like this:

![Make the following changes](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-35.PNG)

```bash
TF=$(mktemp).service
echo '[Service]
Type=oneshot
ExecStart=/bin/sh -c "cat /root/root.txt > /tmp/output"
[Install]
WantedBy=multi-user.target' > $TF
/bin/systemctl link $TF
/bin/systemctl enable --now $TF
```

- `TF=$(mktemp).service`: This line creates a uniquely named temporary file using the mktemp command and assigns it to the variable TF. The .service extension is added so the resulting filename matches the format of a systemd service file.

- `echo '[Service]`: Uses the `echo` command to write the service content to the temporary file created in the before step. The service specifies the type `Type=oneshot` and the command to be executed when the service starts `ExecStart=/bin/sh -c "cat /root/root.txt > /tmp/output"`. This command copies the contents of `/root/root.txt` to `/tmp/output`.

- `[Install]` = tells systemd "When should I run this service automatically?" `enable` = systemd reads `[Install]` to know how to set that up

- `/bin/systemctl link $TF`: This command links the temporary service file to the system startup using `systemctl link`. Doing so allows systemd to recognize and manage the service as a valid service.

- `WantedBy=multi-user.target' > $TF`: `WantedBy=multi-user.target` This is a line you'd usually find inside the [Install] section of a systemd unit file. It tells systemd: “Enable this service to start when the system reaches multi-user.target (a common runlevel for full system operations).”
`' > $TF` `'` closes the quote of a previous echo command (like echo '[Install]). `>` means “write this to a file”. `$TF` is a shell variable that stores the path to a temporary file

- `/bin/systemctl enable --now $TF`: Finally, this command `enable` and starts `--now` the newly created service. The service is executed immediately after being enabled, running the command specified in step 2.


This script creates a temporary service that will execute the command `cat /root/root.txt > /tmp/output` when started, effectively copying the contents of `/root/root.txt` into `/tmp/output`.
Then it links this service to the system startup and enables it, which results in the command running right away as if it were part of the system startup.

- We have to execute the code one by one.

- Once we have executed all the lines of code, a file named `output` will have been created in `/tmp` containing the `root flag`.

- Then we can change to directory `/tmp` and cat the `output` file:
```bash
cd /tmp
ls
cat output
```

![Root flag](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-36.PNG)

2. **What is the root flag value?**

- a58ff8579f0a9270368d33a9966c7fd5 ✅ Correct Answer

**Congratulations, you have completed the Room!!!**

![Congratulations](https://raw.githubusercontent.com/fartaviao/tryhackme-vulnversity/refs/heads/main/Screenshots/Screenshot-37.PNG)

---
## Conclusion and Additional Resources

### Summary
By following these steps, we successfully exploited the **Vulnversity** machine, covering reconnaissance, directory enumeration, web exploitation, and privilege escalation. 🎯

### Recommended Resources:
- TryHackMe Official Documentation → [https://tryhackme.com/](https://tryhackme.com/)
- OpenVPN Documentation → [https://openvpn.net/](https://openvpn.net/)
- TryHackMe safe VPN access → [https://github.com/fartaviao/tryhackme-tutorial/blob/main/Tutorial.md](https://github.com/fartaviao/tryhackme-tutorial/blob/main/Tutorial.md)
- GTFOBins – Unix Privilege Escalation Techniques [GTFOBins](https://gtfobins.github.io/)
- pentestmonkey [GitHub](https://github.com/pentestmonkey)

### Security Considerations
- Always **disconnect the VPN** after finishing a session.
- Use **firewall rules** to prevent unauthorized access.

---

## Author
Created by **Fausto Artavia Ocampo** for educational use and cybersecurity training.

Happy hacking! 🚀
