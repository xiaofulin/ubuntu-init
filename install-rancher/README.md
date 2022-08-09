###### 1.在rancher-server节点运行下面的命令
```shell
# 1.启动容器,等待部署完成
sudo docker run -d --restart=unless-stopped -p 80:80 -p 443:443 -v /etc/localtime:/etc/localtime -v /mnt/code/rancher_data:/var/lib/rancher/ --name rancher --privileged rancher/rancher:stable
# 2.访问容器，获取登陆密码
docker logs  rancher  2>&1 | grep "Bootstrap Password:"
```
###### 2.rancher安装完成，点击新增k8s集群，选择需要部署的组建，在对应的集群角色节点执行添加机器名字

###### 3.在k8s集群安装kubectl命令行工具
```shell
# 1.下载kubectl可执行文件
curl -LO https://dl.k8s.io/release/v1.23.6/bin/linux/amd64/kubectl
```
# 2.安装kubectl
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# 3.将rancher集群对应的k8s集群配置文件放到k8s-master节i点
```shell
mkdir ~/.kube
vim ~/.kube/config
```
kubeconfig
```shell
apiVersion: v1
kind: Config
clusters:
- name: "dev-test"
  cluster:
    server: "https://192.168.1.40/k8s/clusters/c-6lns4"
    certificate-authority-data: "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUJwekNDQ\
      VUyZ0F3SUJBZ0lCQURBS0JnZ3Foa2pPUFFRREFqQTdNUnd3R2dZRFZRUUtFeE5rZVc1aGJXbGoKY\
      kdsemRHVnVaWEl0YjNKbk1Sc3dHUVlEVlFRREV4SmtlVzVoYldsamJHbHpkR1Z1WlhJdFkyRXdIa\
      GNOTWpJdwpPREE0TURneU9EUTRXaGNOTXpJd09EQTFNRGd5T0RRNFdqQTdNUnd3R2dZRFZRUUtFe\
      E5rZVc1aGJXbGpiR2x6CmRHVnVaWEl0YjNKbk1Sc3dHUVlEVlFRREV4SmtlVzVoYldsamJHbHpkR\
      1Z1WlhJdFkyRXdXVEFUQmdjcWhrak8KUFFJQkJnZ3Foa2pPUFFNQkJ3TkNBQVJjK1NXQVFSdjhiU\
      WlhNnlGU1c3OThWRzdPNWRDWENrNzNRZ1BBQlN4bwpqd1R5TlhCKy9vRTd0aXhhZktkNVlUbHdtY\
      kk4UHhSUFBKKzZNdEZEcGtidW8wSXdRREFPQmdOVkhROEJBZjhFCkJBTUNBcVF3RHdZRFZSMFRBU\
      UgvQkFVd0F3RUIvekFkQmdOVkhRNEVGZ1FVQnJXTzhWSkZYOHI0alZFTUN0cEgKTTExZUlsQXdDZ\
      1lJS29aSXpqMEVBd0lEU0FBd1JRSWdNTGx2d1hYN1JIRGxpNlFsaVk2dUpUWTJMcnErYllpWApMb\
      VRINTVoVEJhQUNJUUNLOFNxU0d1b2l1WVZpeHBYck5JTlJTN2V6azhCdmJHZXYzTy9KYWlpazBBP\
      T0KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQ=="
- name: "dev-test-rancher-dev-test-master"
  cluster:
    server: "https://192.168.1.60:6443"
    certificate-authority-data: "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUM0VENDQ\
      WNtZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFTTVJBd0RnWURWUVFERXdkcmRXSmwKT\
      FdOaE1CNFhEVEl5TURnd09EQTROREUxTjFvWERUTXlNRGd3TlRBNE5ERTFOMW93RWpFUU1BNEdBM\
      VVFQXhNSAphM1ZpWlMxallUQ0NBU0l3RFFZSktvWklodmNOQVFFQkJRQURnZ0VQQURDQ0FRb0NnZ\
      0VCQU5jZHM5WU5BTy9IClNSMm5pUVIwN1NjNGlBMTZaOEpFWmxiK2k0azNkbFRVYnBMSmRlNW9PY\
      01Kc1lZc2o3RnJsTC8wVFl4TmpMMjQKSE92bDZSWEFGVmdQVDRDZnpLVGNRRDNtR2FYV21waXR2M\
      FZOcVovdVdnZGhQOHRuSFdNaGdtb1RDUld1THc5ZQpTd2VQZUxQZlZFaEZnaDRGQkk5cWptcHJWN\
      EhRWHh0VEFWNGFCNk5mVS8rbXVlUjJQelBOZmNDZWk1aVZNVWRWCkxOMVFsajZtUEVMbWQ5Y01yd\
      1locHIrazU0R0dBSXBtT0dkR0JCclZqMmpYTGtOdDV0T0NXc3Y4WlF3U2xpYmYKWTJWU29uNVA2N\
      EgvTlJPVmNXUW9XQ3VnMStyTGJkZkFRNmUvWTQrUmFmd3pBbEtaOW1Na2ZtajgzL3Z6dFBSeQpkV\
      2FPajNTdCtDMENBd0VBQWFOQ01FQXdEZ1lEVlIwUEFRSC9CQVFEQWdLa01BOEdBMVVkRXdFQi93U\
      UZNQU1CCkFmOHdIUVlEVlIwT0JCWUVGS2lKWmYrT0ZZL0VtWEVMczVFWk51aERWUmJVTUEwR0NTc\
      UdTSWIzRFFFQkN3VUEKQTRJQkFRQWFCVElSVjhscDV6OUkzM25oVnJHNnVZM3VhZm4wOW1kVEpaW\
      UdETWNpYlJxdGJNVWRhQ09lQ3IrdQp5RTdLR1lybjV2eWZxZE5XbHQrNVFBeDZENDVkOW5scEFEM\
      jRMeEU1bEREZzZqWHVHRFZ0aG5za2JiQlZzV0FZCkUvZG51akR3LzlXYWlCeExiUytSdWN1U3NLb\
      FJDdEh5TG8weVRXWnJiTlp4VFRoTzJKT0djdkdjekppM0xDUVcKeC81RytoK3NTdUM3Z3B1MzBvb\
      jBzaFYwRFlVSnhnZHo1V3BYMnYzOFlxSlppUHc4SnpIYmdUczlmR2FKMTBuVApadWpzQ3ZTRTBDW\
      jdVb2xZeHJ2Mk0zczFucStDVHo4WU5IZC83M0ZRaVF0bjFMa09JcVpBQXdXeDBycXUxa3l3Ck55Q\
      UNDei96T3BOMkFEZ0VBNmdad0xIRjRvOWkKLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo="

users:
- name: "dev-test"
  user:
    token: "kubeconfig-user-vzkz6mqvsc:5m66rrbw8x98xjzffklmzbdpvr7l5r9kglb5wjpdk5d4r9jjm6w4sg"


contexts:
- name: "dev-test"
  context:
    user: "dev-test"
    cluster: "dev-test"
- name: "dev-test-rancher-dev-test-master"
  context:
    user: "dev-test"
    cluster: "dev-test-rancher-dev-test-master"

current-context: "dev-test"

```

###### 4.等待k8s集群安装完成，新建storageclass对象
sc.yaml
```shell
## 创建了一个存储类
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: nfs-storage
  annotations:
    storageclass.kubernetes.io/is-default-class: "true"
provisioner: k8s-sigs.io/nfs-subdir-external-provisioner
parameters:
  archiveOnDelete: "true"  ## 删除pv的时候，pv的内容是否要备份
 
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nfs-client-provisioner
  labels:
    app: nfs-client-provisioner
  # replace with namespace where provisioner is deployed
  namespace: default
spec:
  replicas: 1
  strategy:
    type: Recreate
  selector:
    matchLabels:
      app: nfs-client-provisioner
  template:
    metadata:
      labels:
        app: nfs-client-provisioner
    spec:
      serviceAccountName: nfs-client-provisioner
      containers:
        - name: nfs-client-provisioner
          image: registry.cn-hangzhou.aliyuncs.com/lfy_k8s_images/nfs-subdir-external-provisioner:v4.0.2
          # resources:
          #    limits:
          #      cpu: 10m
          #    requests:
          #      cpu: 10m
          volumeMounts:
            - name: nfs-client-root
              mountPath: /persistentvolumes
          env:
            - name: PROVISIONER_NAME
              value: k8s-sigs.io/nfs-subdir-external-provisioner
            - name: NFS_SERVER
              value: 192.168.1.40 ## 指定自己nfs服务器地址
            - name: NFS_PATH  
              value: /mnt/k8s_nfs/dev-test-pv  ## nfs服务器共享的目录
      volumes:
        - name: nfs-client-root
          nfs:
            server: 192.168.1.40
            path: /mnt/k8s_nfs/pv
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: nfs-client-provisioner
  # replace with namespace where provisioner is deployed
  namespace: default
---
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: nfs-client-provisioner-runner
rules:
  - apiGroups: [""]
    resources: ["nodes"]
    verbs: ["get", "list", "watch"]
  - apiGroups: [""]
    resources: ["persistentvolumes"]
    verbs: ["get", "list", "watch", "create", "delete"]
  - apiGroups: [""]
    resources: ["persistentvolumeclaims"]
    verbs: ["get", "list", "watch", "update"]
  - apiGroups: ["storage.k8s.io"]
    resources: ["storageclasses"]
    verbs: ["get", "list", "watch"]
  - apiGroups: [""]
    resources: ["events"]
    verbs: ["create", "update", "patch"]
---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: run-nfs-client-provisioner
subjects:
  - kind: ServiceAccount
    name: nfs-client-provisioner
    # replace with namespace where provisioner is deployed
    namespace: default
roleRef:
  kind: ClusterRole
  name: nfs-client-provisioner-runner
  apiGroup: rbac.authorization.k8s.io
---
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: leader-locking-nfs-client-provisioner
  # replace with namespace where provisioner is deployed
  namespace: default
rules:
  - apiGroups: [""]
    resources: ["endpoints"]
    verbs: ["get", "list", "watch", "create", "update", "patch"]
---
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: leader-locking-nfs-client-provisioner
  # replace with namespace where provisioner is deployed
  namespace: default
subjects:
  - kind: ServiceAccount
    name: nfs-client-provisioner
    # replace with namespace where provisioner is deployed
    namespace: default
roleRef:
  kind: Role
  name: leader-locking-nfs-client-provisioner
  apiGroup: rbac.authorization.k8s.io

```
