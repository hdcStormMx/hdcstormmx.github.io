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

scp ssh:
--------
-> scp -P 22222 user@serverip:/opt/wireguard-server/config/peer1/peer1.conf .  (copy server to local)  

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

-> sudo apt-get install -y apt-transport-https ca-certificates curl gnupg2 software-properties-common vim fail2ban ntfs-3g

fail2ban:
---------
[sshd]
bantime = 86400
port    = 22222
logpath = %(sshd_log)s
backend = %(sshd_backend)s
maxretry = 3


-> docker: [https://docs.docker.com/engine/install/ubuntu](https://docs.docker.com/engine/install/ubuntu)  
-> portainer: [https://docs.portainer.io/v/ce-2.9/start/install/server/docker/linux](https://docs.portainer.io/v/ce-2.9/start/install/server/docker/linux)  
