# vagrant使用

## 定制镜像

为方便应用部署，通常需要将一些基础的软件包及配置做到镜像中，然后分发镜像。vagrant镜像有两种制作方法：

* 基于现有的镜像修改：先下载可用的镜像，然后启动虚拟机，并进入虚拟机修改配置，安装所需的软件包；然后关机，并通过package命令打包成新的模板
* 

### 基于现有的镜像修改

基于现有镜像修改相对简单，甚至可以做成脚本自动实现；

1. 基于现有的镜像创建虚拟机
2. 修改配置、安装脚本
```bash
# 例如
## setup ssh
sed -e 's/^#PermitRootLogin .*$/PermitRootLogin yes/g' \
    -e 's/^PasswordAuthentication .*$/PasswordAuthentication yes/g' \
    -e 's/^#UseDNS .*/UseDNS no/g' \
    -e 's/^GSSAPIAuthentication .*/GSSAPIAuthentication no/g' \
    -i /etc/ssh/sshd_config
systemctl restart sshd

# disable selinux
setenforce 0
sed -i -e 's/SELINUX=.*/SELINUX=disabled/g' /etc/selinux/config
systemctl stop firewalld
systemctl disable firewalld

# disable network manager
systemctl stop NetworkManager
systemctl disable NetworkManager

# set timezone
timedatectl set-timezone Asia/Shanghai
```
3. 关机并打包

```bash
# vagrant halt
# vagrant package
```
`vagrant package`命令执行后，会生成一个名为`package.box`的文件。这就是新的模板文件。

4. 添加到本次box

将上面生成的文件导入到本地的box仓库，后续就可以使用了。
```bash
# vagrant box add --name centos7 ./package.box
```

### 制作全新的镜像

如果现有镜像不能满足需求，就需要重头开始制作。
