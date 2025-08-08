# Internship - Day 3 : Vulnerability Scan with OpenVAS

Vulnerability scanning is a way to check a system for weaknesses that an attacker could exploit. In this case, I used **OpenVAS** to scan a deliberately vulnerable Windows XP virtual machine that I set up locally. Windows XP was chosen because it’s old, unsupported, and basically a playground for security testing.

---

## Target Details

- **OS:** Windows XP   
- **IP Address:** 192.168.56.101  
- **Network Type:** Host-Only  
- **Firewall:** Off (on purpose)  
- **Antivirus:** None (also on purpose)  

This VM was running on VirtualBox with low resources, so it was a bit slow and sometimes laggy during scans.

---

## Steps I Took

1. Installed the OpenVAS scanner on Oracle Virtualbox using the Official "ova" file of this utility.
2. Setup the OpenVAS to detect and be able to communicate with the internal network of the intentionally vulnerable Windows XP vm.
3. Started the Scan for Windows XP. (Takes a decent long time)
4. Obtained the result (available in this repo).

---

#### Interesting Findings

**1. Operating System (OS) End of Life (EOL) Detection**  

- The scan flagged that Windows XP is no longer supported by Microsoft.  
- No security patches are released, which makes it inherently unsafe to use.

**2. DCE/RPC and MSRPC Services Enumeration Reporting**  

- OpenVAS was able to enumerate details about the RPC services running.  
- This could give an attacker a map of available functions to abuse.

**3. TCP Timestamps Information Disclosure**  

- The system is revealing TCP timestamp data.  
- This can sometimes be used for OS fingerprinting or to guess system uptime.

**4. ICMP Timestamp Reply Information Disclosure**  

- The VM responds to ICMP timestamp requests.  
- This can leak information about the system’s time settings and uptime.

---

## ## CVE Findings

**CVE-1999-0524**  

- **IP:** 192.168.56.101  
- **Vulnerability:** Windows shares with no password protection (default or null password)  
- **Severity:** 5.0 (Medium)  
- **Status:** Open (not remediated)  

---

## Mitigation

Since this vulnerability allows unauthenticated access to shared resources, it’s a direct path for data theft or malware infection.  

1. **Set Strong Passwords on All Shares**  
   
   - Avoid null or blank passwords.  
   - Use complex passwords with a mix of upper/lowercase letters, numbers, and symbols.  

2. **Disable Unnecessary File Sharing**  
   
   - If file sharing is not required, turn it off completely.  

3. **Restrict Share Permissions**  
   
   - Limit shared folders to specific, trusted accounts only.  

4. **Enable a Firewall**  
   
   - Block inbound SMB/NetBIOS traffic from untrusted networks.  

5. **Upgrade the Operating System**  
   
   - Windows XP is no longer supported, and modern OS versions handle share permissions more securely by default.  

---

## Why This Matters

Even without logging into the system, OpenVAS picked up multiple critical vulnerabilities. If this machine was on the internet, it would be compromised in minutes. That’s why running unsupported OS versions is a huge security risk.
