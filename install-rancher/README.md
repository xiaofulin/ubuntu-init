###### 1.在master节点运行下面的命令
```shell
# 1.启动容器
sudo docker run -d --restart=unless-stopped -p 80:80 -p 443:443 -v /etc/localtime:/etc/localtime -v /mnt/code/rancher_data:/var/lib/rancher/ --name rancher --privileged rancher/rancher:stable
# 2.访问容器
```
