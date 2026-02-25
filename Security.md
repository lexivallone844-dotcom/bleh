# Day 1 

## Test Overview

- Identify the phases of Penetration Test
- Plan and scope a Penetration Test at an "entry level"
- Write a formal report

## Pen Testing

### Phase 1 : Mission Definition

- Define mission goals and targets
- Determine scope of mission
- Define the entworks that are valid targets
- Determine which machines are valid targets
- Determine the attacks or exploits that are authorized/appropriate
- Define RoE

### Phase 2: Recon

- Gather info about the target through public sources
- Examine websites, job postings, search engines, etc.
- Enumberate withou ttouching the target


### Phase 3: Footprinting

- Accumulate data throug scanning and/or interaction with the target resources


### Phase 4 : Exploitation/Initial Access

- Gain initial foothold into target network

### Phase 5 : Post-exploitation

- Establish persistence
- escalate priveleges
- obfuscate
- cover your tracks
- exfilrate target data

### Phase 6 : Document Mission

  - Document and report mission details


# Exploitation Research

- Find the os that is running on the computer and be able to do OSINT reasearch on it.

- Ex: you run uname -a (Linux) or ip config /all (Windows) and get a  **ubuntu 4.8.0-41 kernel**, you will just google **ubuntu 4.8.0-41 kernel vulnerabilities**


## Jump Box ssh login

ssh demo1@10.50.16.136 -L 6574:10.208.50.61:80

http://127.0.0.1:6574/calssinfo.html

user: ALVA-502-M

password: WmiTB04OZ2wo

# SSH Multi-plex (Master)Sockets

**ssh -MS /tmp/jmp**(name wtv but directly what target its going to) **demo1@10.50.16.136  -fN**(creates the socket to connect to os and have seperate term to know where the socket was established)

**ssh -S**(using this socket @ /tmp/jump)**/tmp/jmp a@1**(dummy filler, it automatically fills in your creds from establishing the socket) (enters into os) 

**ssh -S /tmp/jmp -O forward -L 2222:(TGT):22** (ride connection into demo  1 box and forwared connection into first target)

**ssh -S /tmp/jmp -O forward -D 9050 a@1** (setting up a dynamic tunnel to machine with same user)

**ssh -S /tmp/jmp -O cancel -D 9050 a@1** (destory dynamic tunnel for socket)

-O : options following ssh cmd and is setting up listening port on your machine to target @ port 22

(note: with the -O it gives paramenters and to use to it to get to different box, you put forward, but to cancel session you put -O cancel)

#Master Socket Demo

 Authenticate to jump box, -S create socket file to store sockets, -M uses multiplexing, Keep this open or the conneciton will ide, up to 17 hops deep only using a single connection

 **ssh -MS /tmp/jump user@ip 2>/dev/null**   (built first connection into ip with errors going to null)

## Host Enumeration

- look @ /etc/host, ip addr
- ping sweep - **for i in {1..254} ;do (ping -c 1 192.168.1.$i | grep "bytes from" &) 2>/dev/null ;done**  ( run this from the machine that can touch the network your pinging)

  (you get some ips)

10.208.50.1

10.208.50.42: 
22/tcp open  ssh 
80/tcp open  http

80/tcp open 
| http-enum: 

|   /robots.txt: Robots file

|   /java/: Potentially interesting directory w/ listing on 'apache/2.4.29 (ubuntu)'

|   /path/: Potentially interesting directory w/ listing on 'apache/2.4.29 (ubuntu)'

|_  /uploads/: Potentially interesting directory w/ listing on 'apache/2.4.29 (ubuntu)'

10.208.50.61

10.208.50.200

10.208.50.230

(now do port enumeration with proxychains)

**ssh -S /tmp/demo ANYTHING -O forward -D 9050** (use the folder you created with MS with -D9050 for dynamic tunnel)

**ssh -S /tmp/demo ANYTHING -O cancel -D9050** (tears down the dynamic tunnel)

(verify sockets we built) 

**ss -antlp**

(verify socket file is in /tmp)

**ls /tmp | grep (filename)**

- run the ssh dynamic cmd on your initial box

( grep to see if dynamic is listening)

**ss -ntlp | grep 9050**


 (scan w/ nmap for port enumeration)

 **proxychains nmap 192.168.28.100,105,110**

(scan w/ nc for port)

**proxychains nc -zvv 10.208.50.42 20-80**


 (create a local forward to port of ip and can -L up to 12x)

 **ssh -S /tmp/demo demo -O forward -L 5694:10.208.50.42:22 -L 5695:10.208.50.42:80**

 (verify that it is listening)

 ss -ntlp | grep (port#)


 (going to website after creating the local forward above)

- go to firefox, type **127.0.0.1:-Lport#/robots.txt**   (note: robots.txt allows you a roadmap to pages you potentially want to look at)


(web/http enumeration)

domain want to look at /robots.txt

**proxychains nmap --script=http-enum (ip) -p (port)**

(will get back a list of paths and youll enter the path at the end of the 127.0.0.1:-Lport# in browser)

**You will be using html inspect on webpages to look for things** how to use it **right click on mouse --> inspect**


## Hypothetical you will have to do later

demo
-> T1:(ip:22)
--> network 192.x.x.x

(create new MS to network)

**ssh -MS /tmp/t1file (creds)@127.0.0.1 -p** (port you used for remote os)

- Run ping sweep again


(move port forward after cancelling from last session to new MS session)

**ssh -S /tmp/demo demo -O cancel -D9050**

**ssh -S /tmp/t1file t1 -0 forward -D9050**

-go through the enumeration again and repeat








