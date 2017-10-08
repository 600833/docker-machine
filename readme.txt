docker-machine  create -d kvm --kvm-boot2docker-url /root/bin/boot2docker.iso  --kvm-network reseau123  b2d02
systemctl stop , disable NetworkManager

[root@server23 ~]# cat reseau123.xml
<network>
  <name>reseau123</name>
  <forward mode='bridge'/>
  <bridge name='bridge123'/>
</network>

# net-define reseau123.xml
# net-autostart reseau123

# net-start --network reseau123

# net-list --all

