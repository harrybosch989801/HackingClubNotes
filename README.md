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


