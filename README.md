# Earth VulnHub CTF Walkthrough

##  Objective
Penetration test the "Earth" virtual machine from VulnHub using Kali Linux. Identify, exploit, and escalate privileges to root through reconnaissance, web application analysis, decryption, and privilege escalation techniques.

##  Tools & Techniques
- Kali Linux
- arp-scan
- Nmap (default, version, full port)
- Gobuster
- CyberChef (XOR decryption)
- Netcat
- ltrace (privilege escalation debugging)
- Linux command-line basics (cat, whoami, chmod, etc.)

##  Summary of Steps

### 1. Network Discovery
- Used `sudo arp-scan -l` to find the IP of the Earth VM.

### 2. Port Scanning
- Used `nmap -sV -Pn` and later `nmap -p- -sC -sV -T4` to reveal ports 22, 80, and 443.
- Discovered domains: `earth.local` and `terratest.earth.local`.

### 3. Host File Configuration
- Edited `/etc/hosts` to resolve the domains in a browser.

### 4. Web Enumeration
- Visited `earth.local` and observed encrypted messages.
- Used Gobuster to find `/admin` login page.

### 5. Second Domain Investigation
- Found `robots.txt` and `testingnotes.txt` under `terratest.earth.local`.
- Learned encryption method: XOR.

### 6. Decryption with CyberChef
- Used XOR decryption to uncover password: `earthclimatechangebad4humans`

### 7. Web Login
- Logged into admin panel with `terra:earthclimatechangebad4humans`
- Found and read `user_flag.txt`

### 8. Reverse Shell Access
- Used base64 encoded command to create a reverse shell via Netcat.
- Gained shell access as `apache` user.

### 9. Privilege Escalation
- Discovered `reset_root` and explored with `ltrace`
- Created expected access files, reran reset_root
- Retrieved root password: `Earth`

### 10. Root Access
- Switched user to root, confirmed with `whoami`
- Captured final flag

## Lessons Learned
- XOR decryption via CyberChef
- Importance of enumerating virtual hosts
- Using tools like ltrace to debug binary behavior
- SUID exploration and reverse shell management

## Resources
- [Earth VulnHub VM](https://download.vulnhub.com/theplanets/Earth.ova)
- [Walkthrough Guide](https://medium.com/@wiktorderda/vulnhub-the-planets-earth-ctf-4d4acfeb1428)

## Full Walkthrough
See `docs/full-walkthrough.md` for detailed step-by-step documentation.
