##  ubuntu 安装后初始化操作


###### 1.允许root登陆
```shell
#1. 切换为root用户
sudo -i

#2. 设置root用户密码,输入两次密码
passwd

#3. 修改ssh服务配置
vi /etc/ssh/sshd_config
PermitRootLogin yes

#4. 重启sshd服务
#sudo service sshd restart

```

###### 2.更改网卡名字
```shell
#1. 修改grub文件
vim /etc/default/grub
GRUB_CMDLINE_LINUX="" 改为 GRUB_CMDLINE_LINUX="net.ifnames=0 biosdevname=0"

#2. 重新生成grub引导配置文件
grub-mkconfig -o /boot/grub/grub.cfg

#3. 修改网络配置文件中的ens32为eth0
vim /etc/netplan/01-netcfg.yaml

#4. 重启
reboot
```

###### 3.设置文件最大打开数
```shell
# 1. 系统
vim /etc/sysctl.conf
# 2. 添加
fs.file-max = 65535

sysctl -p

# 用户
vim /etc/security/limits.conf
# 添加
*               hard    nofile          65535
*               soft    nofile          65535
root            hard    nofile          65535
root            soft    nofile          65535

# Systemd
vim /etc/systemd/system.conf
# 添加
DefaultLimitCORE=infinity
DefaultLimitNOFILE=65535
DefaultLimitNPROC=65535
systemctl daemon-reexec
```

####### 4.配置阿里云源
mv /etc/apt/sources.list /etc/apt/sources.list_bak

vim /etc/apt/sources.list

deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse


apt update


###### 5.同步时区和安装docker
```shell
timedatectl set-timezone Asia/Shanghai

echo “LC_TIME=en_DK.UTF-8” >> /etc/default/locale

hostnamectl set-hostname k8s-master

swapoff -a
sed -ri 's/.*swap.*/#&/' /etc/fstab


vi /etc/hosts
192.168.1.40 rancher
192.168.1.60 k8s-master
192.168.1.61 k8s-node01


vi /etc/docker/daemon.json
{
    "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn","https://registry.docker-cn.com","http://hub-mirror.c.163.com"]
}
```
