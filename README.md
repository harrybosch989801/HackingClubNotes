## 2/4/25

Definitions and Concepts:
* VPN -- Virtual Private Network.  We're using wireguard as our vpn.  There are many ways to structure a vpn network, we are using a simple hub structure.  We have a vm running in the cloud and it acts as our "router" within the vpn.
* Container -- Loosely, a container can be thought of as a lightweight virtual machine that shares a kernel with the host OS

Commands used today:
* pwd  -- Displays the current directory
```
drew@Bosch ~> pwd
/home/drew
drew@Bosch ~>
```
* ls  -- Displays the contents of a directory
```
drew@Bosch ~/w/h/bash_exercises> ls
app/  Dockerfile  hashes/  start.sh*
drew@Bosch ~/w/h/bash_exercises>
```
* echo  -- Prints text to the terminal
```
drew@Bosch ~/w/h/bash_exercises> echo "hello world"
hello world
drew@Bosch ~/w/h/bash_exercises> 
```
* cp  -- copies a file
```
drew@Bosch ~/w/h/bash_exercises> cp start.sh /tmp/junk/start.sh
drew@Bosch ~/w/h/bash_exercises> ls /tmp/junk/
start.sh*
drew@Bosch ~/w/h/bash_exercises> 
```
* sudo -- executes a command with elevated permissions
```
drew@Bosch ~/w/h/bash_exercises> apt update
Error: Could not open lock file /var/lib/apt/lists/lock - open (13: Permission denied)
Error: Unable to lock directory /var/lib/apt/lists/
Warning: Problem unlinking the file /var/cache/apt/pkgcache.bin - RemoveCaches (13: Permission denied)
Warning: Problem unlinking the file /var/cache/apt/srcpkgcache.bin - RemoveCaches (13: Permission denied)
drew@Bosch ~/w/h/bash_exercises [100]> sudo apt update
Hit:1 http://security.ubuntu.com/ubuntu oracular-security InRelease
Hit:2 http://us.archive.ubuntu.com/ubuntu oracular InRelease       
Hit:3 http://us.archive.ubuntu.com/ubuntu oracular-updates InRelease
Hit:4 http://us.archive.ubuntu.com/ubuntu oracular-backports InRelease
3 packages can be upgraded. Run 'apt list --upgradable' to see them.
drew@Bosch ~/w/h/bash_exercises> 
```
* apt -- package manager used to install and update software
```
drew@Bosch ~/w/h/bash_exercises [127]> sudo apt install cowsay
Installing:                     
  cowsay

Suggested packages:
  filters  cowsay-off

Summary:
  Upgrading: 0, Installing: 1, Removing: 0, Not Upgrading: 3
  Download size: 18.6 kB
  Space needed: 93.2 kB / 196 GB available

Get:1 http://us.archive.ubuntu.com/ubuntu oracular/universe amd64 cowsay all 3.03+dfsg2-8 [18.6 kB]
Fetched 18.6 kB in 0s (220 kB/s)  
Selecting previously unselected package cowsay.
(Reading database ... 332683 files and directories currently installed.)
Preparing to unpack .../cowsay_3.03+dfsg2-8_all.deb ...
Unpacking cowsay (3.03+dfsg2-8) ...
Setting up cowsay (3.03+dfsg2-8) ...
Processing triggers for man-db (2.12.1-3) ...
drew@Bosch ~/w/h/bash_exercises> cowsay "hello"
 _______
< hello >
 -------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
drew@Bosch ~/w/h/bash_exercises>
```
* systemctl -- "system control" used to (amongst other things) start/stop/enable/disable services
```
drew@Bosch ~/w/h/bash_exercises> sudo systemctl stop wg-quick@wg0.service
drew@Bosch ~/w/h/bash_exercises> sudo systemctl start wg-quick@wg0.service
drew@Bosch ~/w/h/bash_exercises> systemctl status wg-quick@wg0
● wg-quick@wg0.service - WireGuard via wg-quick(8) for wg0
     Loaded: loaded (/usr/lib/systemd/system/wg-quick@.service; disabled; preset: enabled)
     Active: active (exited) since Tue 2025-02-04 22:18:39 EST; 9s ago
 Invocation: e2dfcbd8bbe04ae990e7005340a5fc7d
       Docs: man:wg-quick(8)
             man:wg(8)
             https://www.wireguard.com/
             https://www.wireguard.com/quickstart/
             https://git.zx2c4.com/wireguard-tools/about/src/man/wg-quick.8
             https://git.zx2c4.com/wireguard-tools/about/src/man/wg.8
    Process: 6307 ExecStart=/usr/bin/wg-quick up wg0 (code=exited, status=0/SUCCESS)
   Main PID: 6307 (code=exited, status=0/SUCCESS)
   Mem peak: 4.3M
        CPU: 121ms
```
* ping -- sends a packet and asks for a response from another server.  A way of testing connectivity.
```
drew@Bosch ~/w/h/bash_exercises> ping 10.8.0.1
PING 10.8.0.1 (10.8.0.1) 56(84) bytes of data.
64 bytes from 10.8.0.1: icmp_seq=1 ttl=64 time=51.0 ms
64 bytes from 10.8.0.1: icmp_seq=2 ttl=64 time=24.3 ms
^C
--- 10.8.0.1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 24.256/37.604/50.953/13.348 ms
drew@Bosch ~/w/h/bash_exercises>
```


## 2/18/25
Definitions and Concepts:
* Pipes and I/O redirection 
  * Pipes -- pipes in bash redirect stdout from the previous command to stdin on the following command.
  ``` 
  root@wgtest:~# cat /etc/wireguard/wg0.conf | grep -i -v key
  [Interface]
  Address = 10.8.1.102/24

  [Peer]
  AllowedIPs = 10.8.1.0/24
  Endpoint = 129.80.112.211:51820
  PersistentKeepalive = 25
  root@wgtest:~# 
  ```
  The first command printng the contents of wg0.conf to stdout.  This output is instead piped into stdin of grep which is then used to filter out the keys.
  * I/O redirection -- redirect stdout/stderr/stdin to a file or file descriptor
  ``` 
  root@wgtest:/tmp# echo "hello world" > hello.txt
  root@wgtest:/tmp# ls hello.txt 
  hello.txt
  root@wgtest:/tmp#
  ```
  The output of the echo command is written to the hello.txt file.

  ``` 
  root@wgtest:/tmp# find / -name passwd 2> /dev/null
  /etc/pam.d/passwd
  /etc/passwd

  ```
  The error output from the find command is sent to /dev/null (the linux black hole)
* Reverse Shells -- In the Linux world, logging into a machine is colloquially referred to as "getting a shell".  Consequently, a "reverse" shell is when we force the machine we want to log into, to connect back to us.  I.e. the process is in reverse.
  Once we get some kind of command injection on a target machine, a typical next step would be to try and execute a reverse shell to establish persistent access.  Next meetup we'll talk a lot more about reverse shells.
  https://swisskyrepo.github.io/InternalAllTheThings/cheatsheets/shell-reverse-cheatsheet/#bash-tcp
  
* Hashing -- an irreversible one-way function that changes data into a fixed size blob of text.  Typically in the context of computers this is used for passwords, verifying data integrity, or indexing data (i.e. in a hashmap).  There are many different hashing algorithms.  Today we briefly talked about md5sum which is a weak algorithm for passwords, but useful for data integrity checks.

Commands used today:
* md5sum -- computes md5sum hash of stdin
```
root@wgtest:~# cat /etc/wireguard/wg0.conf |md5sum
649bf03ad8aead657d7a9dd40998737e  -
root@wgtest:~#
```
* head -- sends the first n lines of stdin to stdout
```
root@wgtest:~# cat /etc/passwd | head -n 3
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
root@wgtest:~# 
```
* tail -- sends the last n lines of stdin to stdout
```
root@wgtest:~# tail -n 3 /etc/passwd
fwupd-refresh:x:990:990:Firmware update daemon:/var/lib/fwupd:/usr/sbin/nologin
polkitd:x:989:989:User for polkitd:/:/usr/sbin/nologin
ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
root@wgtest:~# 
```
* find -- finds files in the system.  has a million options...  just google if you need anything fancy :)
```
root@wgtest:~# find / -name passwd 2>/dev/null
/etc/pam.d/passwd
/etc/passwd
/usr/bin/passwd
/usr/share/bash-completion/completions/passwd
/usr/share/doc/passwd
/usr/share/lintian/overrides/passwd
root@wgtest:~#
```
* grep -- parse stdin using regular expressions/pattern matching.  very powerful tool with practice it will become your good friend.  Worth a google and some experimentation.
```
root@wgtest:~# grep root /etc/passwd   
root:x:0:0:root:/root:/bin/bash      
root@wgtest:~# cat /etc/passwd | grep -i 'root\|ubuntu'        
root:x:0:0:root:/root:/bin/bash              
ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash         
root@wgtest:~#

drew@drahsin ~> grep '^.i.or$' /usr/share/dict/words
Timor
minor
rigor
vigor
visor
vizor
drew@drahsin ~>
```
* ufw -- uncomplicated firewall.  default firewall interface for ubuntu.  details not important for now.  we simply used to disable/re-enable firewall
* sort -- sorts stdin and sends it to stdout
```
root@wgtest:/usr/share/dict# sort /etc/passwd |head -n 3
_apt:x:42:65534::/nonexistent:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
root@wgtest:/usr/share/dict#
```
* tr -- trims/deletes text
* shuf -- choose n lines at random from stdin
* cut -- another tool for trimming/deleting text
```
drew@drahsin ~/w/h/b/hashes> shuf -n 1 rockyou.txt |tr -d '\n' | md5sum|cut -d' ' -f1 
3234f55b5b79bd36b6cdc559c55f3644
drew@drahsin ~/w/h/b/hashes>
```
We use shuf to select a single record from the file rockyou.txt.  We send that line to tr and trim off the newline character.  Then we take the md5sum.  Then we tokenize the output on spaces and select the first field.
