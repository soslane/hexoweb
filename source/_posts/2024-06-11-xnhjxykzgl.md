---
layout:       post
title:        "虚拟化技术 镜像与快照管理"
author:       "Gweek"
header-style: text
catalog:      true
tags:
    - 虚拟化   

---

### 一、增量镜像

在KVM虚拟化环境中，增量镜像是一种非常有用的技术，它允许你在一个基础镜像的基础上创建多个子镜像，每个子镜像都只记录自己的修改，从而节省磁盘空间并快速复制虚拟机。

导入cirros的相关镜像（cirros-0.5.1-x86_64-disk.img）创建一个名称为vm1的虚拟机。

实验环境：上传镜像文件cirros-0.5.1-x86_64-disk.img至/vm目录下

`cd /vm`  进入/vm目录下

`ls`  查看/vm目录下的文件

`mv cirros-0.5.1-x86_64-disk.img cirros.qcow2`  将镜像文件改名为cirros.qcow2

`qemu-img info cirros.qcow2`  查看镜像文件信息

由于实验要求在虚拟机中创建一个200M的文件，所以在本实验中要对镜像文件进行扩容

```bash
qemu-img resize cirros.qcow2 +900M
```

`qemu-img info cirros.qcow2`  再次查看镜像文件信息，看是否扩容成功

`virt-install --name cirros --vcpu=1 --ram=128 --import --disk path=cirros.qcow2 --network network=default --os-type=linux`  利用导入现有镜像文件创建一个名为cirros的虚拟机

`qemu-img create -b cirros.qcow2 -f qcow2 cirros1.qcow2` 创建cirros1.qcow2增量镜像文件

`qemu-img info cirros1.qcow2` 查看增量镜像文件信息

`cp /etc/libvirt/qemu/cirros.xml /etc/libvirt/qemu/cirros1.xml` 复制一份cirros的配置文件并命名为cirros1.xml

`vi /etc/libvirt/qemu/cirros1.xml` 修改cirros1的配置文件

修改参数 name:cirros1 / uuid / source file / mac地址

`virsh define /etc/libvirt/qemu/cirros1.xml` 利用virsh define注册虚拟机

`virsh start cirros1` 启动cirros1虚拟机

`virsh list --all` 查看虚拟机

打开virt-manager进入cirros1虚拟机，并输入用户名密码进入系统

`sudo if=/dev/zero of=test bs=1M count=200` 创建一个200M的test文件

`qemu-img info cirros1.qcow2`  回到finalshell中查看增量镜像信息

`qemu-img commit cirros1.qcow2`  合并增量镜像

`qemu-img info cirros.qcow2` 查看后备镜像 完成增量镜像合并

### 二、qcow2镜像加密

对QCOW2镜像文件进行加密是一种保护数据安全的方法。可以使用加密功能来确保只有授权的用户才能访问镜像文件中的数据

AES加密：AES（Advanced Encryption Standard）是一种对称密钥加密算法，它被广泛用于保护数据的安全性。AES算法由美国国家标准与技术研究院（NIST）于2001年发布。AES使用相同的密钥（称为对称密钥）来加密和解密数据，这意味着发送方和接收方必须共享相同的密钥。AES算法支持不同的密钥长度，包括128位、192位和256位。密钥长度越长，加密的安全性就越高，但也会增加计算的复杂性。

实验环境：上传镜像文件cirros-0.5.1-x86_64-disk.img至/vm目录下

`cd /vm`  进入/vm目录下

`ls`  查看/vm目录下的文件

`mv cirros-0.5.1-x86_64-disk.img cirros.qcow2`  将镜像文件改名为cirros.qcow2

`qemu-img info cirros.qcow2`  查看镜像文件信息

`virt-install --name cirros --vcpu=1 --ram=128 --import --disk path=cirros.qcow2 --network network=default --os-type=linux`  利用导入现有镜像文件创建一个名为cirros的虚拟机

`virsh destroy cirros`  将cirros虚拟机关机

`virsh list --all` 查看虚拟机状态

`qemu-img convert -f qcow2 cirros.qcow2 -O qcow2 -o encryption cirrosa.qcow2` 创建一个加密的镜像文件cirrosa.qcow2

输入密码如123456

`qemu-img info cirrosa.qcow2` 查看加密镜像文件信息，确定该镜像已经是加密状态

`vi secret.xml` 创建一个密钥

输入以下内容

```xml
<secret ephemeral='no' private='yes'>
</secret>
```

退出保存

`virsh secret-define secret.xml`  生成UUID并保存

```shell
`MYSECRET=`printf %s "123456" | base64`` 将密码"123456"转换为Base64编码，并将结果保存到了MYSECRET变量中
```
`echo $MYSECRET` 显示MYSECRET变量的值

`virsh secret-set-value 上面生成的UUID $MYSECRET` 设置密钥值

`vi /etc/libvirt/qemu/cirros.xml`  编辑cirros虚拟机的配置文件

修改下列信息

![66c0470cd0811d0271f4e.png](https://img.myla.eu.org/file/66c0470cd0811d0271f4e.png)

`virsh define /etc/libvirt/qemu/cirros.xml`  重新定义cirros

`virsh start cirros `  成功启动虚拟机

