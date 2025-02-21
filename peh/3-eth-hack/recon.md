# Information Gathering

## Passive reconnaissance

➡️ **Physical** engagement / **Social** engineering

- **Location** information like
  - satellite images
  - drone recon
  - building layout (badge readers, security, fencing, etc)
- **Job** information
  - employees (name, job title, phone number, etc)
  - pictures (badge photos, desk photos, computer photos, etc)



➡️ Web / Host Assessment

- target validation
  - `whois`, `nslookup`, `dnsrecon`
- finding subdomains
  - Google, `dig`, `nmap`, `crt.sh`, etc
- fingerprinting
  - `nmap`, `wappalyzer`, `netcat`, etc
- data breaches
  - [HaveIBeenPwned](https://haveibeenpwned.com/), Breach-Parse, WeLeakInfo



### Target

> ❗ Always refer to a Bug Bounty program to find valid targets that can be legally tested
>
> 🔗 [Bugcrowd](https://bugcrowd.com/engagements)
>
> - 🧪  `e.g.` - [Tesla](https://bugcrowd.com/tesla)

- Read the **program details**, follow the terms and stay in scope
- Following test will be made on the `*.tesla.com` target

---

## Discovering email addresses

- The goal is discovering public email addresses and check if they really exist

➡️ [Hunter.io](https://hunter.io/domain-search) (free registration) - Find email addresses from any company name or website

![Hunter.io](.gitbook/assets/2024-07-02_20-03-06_537.png)

➡️ [Phonebook.cz](https://phonebook.cz/) (free registration) - Phonebook lists all domains, email addresses, or URLs for the given input domain

➡️ [VoilaNorbert](https://www.voilanorbert.com/)

➡️ [Clearbit Connect](https://clearbit.com/resources/tools/connect) (Chrome extension)

➡️ [EmailHippo Email address verifiy](https://tools.emailhippo.com/) - Free email address verification tool

➡️ [Email-checker](https://email-checker.net/)

---

## Breached credentials

➡️ [HaveIBeenPwned](https://haveibeenpwned.com/) - Check if your email address is in a data breach

➡️ [breach-parse](https://github.com/hmaverickadams/breach-parse) - A tool for parsing breached passwords

- `BreachCompilation` password list (44GB) file comes from breached password dumps

```bash
breach-parse @tesla.com tesla.txt "~/Downloads/BreachCompilation/data"
```

**Credential stuffing** and **Password spraying** can be done using the results.



➡️ [DeHashed.com](https://dehashed.com/) (subscription) - public data search-engine

- Hashed passwords or other data can be found
- Collect all the data (email, username, IP, address, etc) with the goal to find patterns, that could be related to personal accounts too
- Investigation to tie the data to other accounts, etc
- Use tools to try to decrypt the hashed password, like [Hashes.com](https://hashes.com/en/decrypt/hash), Google, etc

![DeHashed.com](.gitbook/assets/2024-07-02_20-30-23_539.png)

---

## Hunting subdomains

Identify subdomains 

➡️ [Sublist3r](https://github.com/aboul3la/Sublist3r) (outdated) - enumerate subdomains of websites using OSINT

```bash
sudo apt install sublist3r
```

```bash
sublist3r -d tesla.com

sublist3r -d tesla.com -t 100 -v
```



➡️ [crt.sh](https://crt.sh/) - look for registered certificates and find subdomains or sub-subdomains

![crt.sh](.gitbook/assets/2024-07-02_23-58-39_542.png)

➡️ [amass](https://github.com/owasp-amass/amass) - in-depth attack surface mapping and asset discovery

```bash
sudo apt install amass
```

```bash
amass enum -d tesla.com

amass enum -d syselement.com
```

![amass enum -d tesla.com](.gitbook/assets/2024-07-03_00-09-39_543.png)

![amass enum -d syselement.com](.gitbook/assets/2024-07-03_00-12-42_544.png)

➡️ [httprobe](https://github.com/tomnomnom/httprobe) - take a list of domains and probe for working (alive) http and https servers

```bash
# Go is necessary (installed via pimpmykali.sh)
go install github.com/tomnomnom/httprobe@latest

# or on Kali
sudo apt install httprobe
```

```bash
cat tesla.com/recon/final.txt | httprobe

# Skip default probes, and use only https:443 probe
cat tesla.com/recon/final.txt | httprobe -s -p https:443

# Strip only subdomains from the list
cat tesla.com/recon/final.txt | sort -u | httprobe -s -p https:443 | sed 's/https\?:\/\///' | tr -d ':443'
```

➡️ [assetfinder](https://github.com/tomnomnom/assetfinder) - find domains and subdomains related to a given domain

```bash
# Go is necessary (installed via pimpmykali.sh)
go get -u github.com/tomnomnom/assetfinder

# or on Kali
sudo apt install assetfinder
```

```bash
assetfinder syselement.com

assetfinder --subs-only tesla.com
```

---

## Screenshotting websites

➡️ [gowitness](https://github.com/sensepost/gowitness) - A golang, web screenshot utility using Chrome Headless

```bash
# Go is necessary (installed via pimpmykali.sh)
go install github.com/sensepost/gowitness@latest

# or on Kali
sudo apt install gowitness
```

```bash
gowitness scan single --url "https://tesla.com" --write-db

gowitness scan single --url "https://blog.syselement.com"
```

---

## Website technologies

➡️ [BuiltWith.com](https://builtwith.com/) - find out what websites are built with

![BuiltWith.com](.gitbook/assets/2024-07-03_19-54-58_561.png)

➡️ [Wappalyzer.com](https://www.wappalyzer.com/) - via browser extension

- by visiting the webpage, interact with the browser extension to check the website technologies

![](.gitbook/assets/2024-07-03_20-14-16_570.png)

➡️ [WhatWeb](https://github.com/urbanadventurer/WhatWeb/)

```bash
whatweb https://blog.syselement.com/
```

![](.gitbook/assets/2024-07-03_20-17-06_571.png)

---

## Automated recon script

- Little `bash` script for sub-domains hunting

```bash
#!/bin/bash

url=$1

if [ ! -d "$url" ]; then
	mkdir $url
fi

if [ ! -d "$url/recon" ]; then
	mkdir $url/recon
fi

# Assetfinder #
echo "[+] Harvesting subdomains with assetfinder..."
assetfinder $url >> $url/recon/assets.txt
# get only subdomains containing $url
cat $url/recon/assets.txt | grep $1 >> $url/recon/final.txt
rm $url/recon/assets.txt

# Amass #
# echo "[+] Harvesting subdomains with amass..."
# amass enum -d $url >> $url/recon/f.txt
# sort -u $url/recon/f.txt >> $url/recon/final.txt
# rm $url/recon/f.txt

# httprobe #
echo "[+] Probing for alive domains..."
cat $url/recon/final.txt | sort -u | httprobe -s -p https:443 | sed 's/https\?:\/\///' | tr -d ':443' >> $url/recon/alive.txt

###
```

➡️ [sumrecon](https://github.com/Gr1mmie/sumrecon/blob/master/sumrecon.sh) - web recon script

```bash
wget https://raw.githubusercontent.com/Gr1mmie/sumrecon/refs/heads/master/sumrecon.sh
```

- TCM's modified final script
  - **Creates a directory structure** for reconnaissance under a given URL
  - **Harvests subdomains** using `assetfinder`
  - **Filters valid subdomains** and saves them to `final.txt`
  - **Checks for live domains** using `httprobe`
  - **Identifies potential subdomain takeovers** using `subjack`
  - **Scans for open ports** using `nmap`
  - **Scrapes archived URLs** from `waybackurls`
  - **Extracts parameters** from Wayback Machine data
  - **Categorizes JavaScript, PHP, JSON, JSP, and ASPX files** from Wayback Machine data
  - **Removes temporary files** to keep the structure clean
  - (Commented out) **Could run `amass` for subdomain discovery** and **use `EyeWitness` for screenshots**

```bash
# 0. Requirements
sudo apt install amass assetfinder httprobe gowitness nmap subjack
go install github.com/tomnomnom/waybackurls@latest

# 1. Copy the code here https://pastebin.com/raw/MhE6zXVt to a new file

# 2. Fix last 2 lines with gowitness and uncomment them
# echo "[+] Running eyewitness against all compiled domains..."
# gowitness scan file -f $url/recon/httprobe/alive.txt

chmod +x finalrecon.sh
./finalrecon.sh syselement.com
```

> - Check those additional resources
>   - The Bug Hunter's Methodology - [The Bug Hunter's Methodology Full 2-hour Training by Jason Haddix](https://www.youtube.com/watch?v=uKWu6yhnhbQ)
>   - [Nahamsec Recon Playlist](https://www.youtube.com/watch?v=MIujSpuDtFY&list=PLKAaMVNxvLmAkqBkzFaOxqs3L66z2n8LA)

---

## Using Burpsuite

➡️ [Burp Suite](https://portswigger.net/burp/communitydownload)

![](.gitbook/assets/2024-07-03_20-25-14_572.png)

---

## Google Fu

➡️ [Google.com](https://www.google.com/)

- [Google Search Syntax](https://www.google.com/search?client=firefox-b-e&q=google+search+syntax)
- [Google Search Operators: The Complete List (44 Advanced Operators)](https://ahrefs.com/blog/google-advanced-search-operators/)

```bash
site:tesla.com filetype:pdf
```

---

## Social Media

- [Linkedin](https://www.linkedin.com/), [Twiter (X)](https://x.com/) or other public websites can be used for some social media OSINT (Open-Source Intelligence).

---



