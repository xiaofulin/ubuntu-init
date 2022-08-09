```shell
#1.安装nfs服务
server:

sudo apt install nfs-kernel-server
mkdir -p /data/nfs
sudo chown nobody:nogroup /data/nfs
sudo chmod 777 /data/nfs
vi /etc/exports
/data/nfs 192.168.1.62(rw,sync,no_subtree_check)
/data/nfs 192.168.1.63(rw,sync,no_subtree_check)

exportfs -a
sudo systemctl restart nfs-kernel-server
ufw disable 

#2.客户端挂载nfs服务
client:
mkdir -p /data/nfs

apt-get install nfs-common

vi /etc/fstab 
192.168.1.61:/data/nfs /data/nfs nfs defaults 0 0

mount -a

```
