Change pass
-----------
-> sudo passwd "user"  
->	sudo -i  
->	passwd  
   
mkdir:
------
->	mkdir /dir/newdir  
->	mkdir -p /dir/dir/subd1/subd2/subd3  
->	mkir -m 777 /dir/dir  
  
rm / rmdir:
-----------
-> rm filename  
-> rm filename1 filename2 filename3  
-> rm *.pdf  
-> -i (confirm)  
-> -f (force)  
-> rm -d / rmdir (empty directory)  
-> rm -r dirname (recursive)  
  
scp ssh:
--------
Copy server to local  
```
scp -P 22222 user@serverip:/opt/wireguard-server/config/peer1/peer1.conf .
scp -P 22222 user@serverip:/opt/wireguard-server/config/peer1/peer1.png .
```

ssh config:
-----------
-> ssh-keygen -t rsa -b 4096  (create key)  
-> ssh-copy-id -i ~/.ssh/id_rsa.pub user@serverip -p 22222	(copy local to server /home/user/.ssh/authorized_keys)  
-> (nano || vim) /etc/ssh/sshd_config  

-> edit config file:
 - PermitRootLogin no
 - PubkeyAuthentication yes
 - PasswordAuthentication no
 - PermitEmptyPasswords no
 - ChallengeResponseAuthentication no
 - UsePAM yes


new server:
-----------
-> sudo apt update  
-> sudo apt full-upgrade  
-> sudo reboot  

-> `sudo apt-get install -y apt-transport-https ca-certificates curl gnupg2 software-properties-common vim fail2ban ntfs-3g`

fail2ban:
---------
```[sshd]  
bantime = 86400  
port    = 22222  
logpath = %(sshd_log)s  
backend = %(sshd_backend)s  
maxretry = 3  
```
  
```[openvpn]  
enabled  = true  
port     = 1194  
protocol = udp  
filter   = openvpn  
logpath  = /var/log/openvpn.log  
maxretry = 3  
```
  
-> service fail2ban restart  

  
docker / portainer:
-------------------
-> docker: [https://docs.docker.com/engine/install/ubuntu](https://docs.docker.com/engine/install/ubuntu)  
-> portainer: [https://docs.portainer.io/v/ce-2.9/start/install/server/docker/linux](https://docs.portainer.io/v/ce-2.9/start/install/server/docker/linux)  
-> [update-portainer](https://docs.portainer.io/v/ce-2.11/start/upgrade)  
  
WireGuard:
----------
```
version: "2.1"  
services:  
  wireguard:  
    image: linuxserver/wireguard  
    container_name: wireguard  
    cap_add:  
      - NET_ADMIN  
      - SYS_MODULE  
    environment:  
      - PUID=1000  
      - PGID=1000  
      - TZ=Europe/London  
      - SERVERURL=???.duckdns.org #optional  
      - SERVERPORT=51820 #optional  
      - PEERS=1 #optional  
      - PEERDNS=auto #optional  
      - INTERNAL_SUBNET=10.13.13.0 #optional  
    volumes:  
      - /opt/wireguard-server/config:/config  
      - /lib/modules:/lib/modules  
    ports:  
      - 51820:51820/udp  
    sysctls:  
      - net.ipv4.conf.all.src_valid_mark=1  
    restart: always  
```


