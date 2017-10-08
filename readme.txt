[root@server23 ~]# cat reseau123.xml
<network>
  <name>reseau123</name>
  <forward mode='bridge'/>
  <bridge name='bridge123'/>
</network>

[root@server23 network-scripts]# cat ifcfg-bridge123
DEVICE=bridge123
NAME=bridge123
ONBOOT=yes
TYPE=Bridge
BOOTPROTO=none
STP=yes
IPADDR=192.168.123.123
NETMASK=255.255.255.0
GATEWAY=192.168.123.1
DNS1=192.168.123.1

[root@server23 network-scripts]# cat ifcfg-eth0
DEVICE=eth0
NAME=eth0
ONBOOT=yes
TYPE=ethernet
BOOTPROTO=none
BRIDGE=bridge123


# net-define reseau123.xml
# net-autostart reseau123

# net-start --network reseau123

# net-list --all

docker-machine  create -d kvm --kvm-memory "3000" --kvm-cpu-count "2" --kvm-boot2docker-url /root/bin/boot2docker.iso  --kvm-network reseau123  b2d02
echo "ip addr add 192.168.123.201/24 dev eth0" | docker-machine ssh b2d02 sudo tee /var/lib/boot2docker/bootsync.sh > /dev/null
systemctl stop , disable NetworkManager

docker-machine ssh b2d02 sudo /etc/init.d/docker stop
docker-machine ssh b2d02 sudo rm -rf /var/lib/docker/*
vi /var/lib/boot2docker/profile
EXTRA_ARGS='
--label provider=kvm 
--insecure-registry=registry.loc:5000
'
CACERT=/var/lib/boot2docker/ca.pem 
DOCKER_HOST='-H tcp://0.0.0.0:2376'
DOCKER_STORAGE=overlay2
DOCKER_TLS=auto                              
SERVERKEY=/var/lib/boot2docker/server-key.pem
SERVERCERT=/var/lib/boot2docker/server.pem


root@b2d02:~# cat /var/lib/boot2docker/bootsync.sh 
ip addr add 192.168.123.201/24 dev eth0
echo '192.168.123.122 registry.loc' >> /etc/hosts


[root@server22 deploy]# docker swarm  join-token worker
To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-1unv1hizfkmstndlg0ebjrem8v4o7tbixbyh9hxsrm9zudb9kt-80ow75hqith57tmp41j4cod86 192.168.123.122:2377
