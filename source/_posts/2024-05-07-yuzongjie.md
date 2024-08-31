---
layout:       post
title:        "Windows server 域服务基础知识"
author:       "Gweek"
header-style: text
catalog:      true
tags:
    - Windows server

---

#### **1、概述**

活动目录域服务 (AD DS) 是 Windows Server 操作系统中的核心身份验证和授权服务。它提供了一个集中存储和管理网络资源信息的位置，例如用户、计算机、组和应用程序。

#### 2、AD DS(活动目录域服务）的主要功能

1、身份验证和授权：AD DS 用于验证用户身份并授权他们访问网络资源。 2、集中管理：AD DS 提供了一个集中位置来管理网络资源。 3、资源共享：AD DS 使得在网络上共享资源更加容易。 4、安全：AD DS 提供多种安全功能来保护您的网络。

#### **3、全局编录 (GC)** 

是 Active Directory 域服务 (AD DS) 中的一项功能，它在单个域控制器上存储域中所有对象的只读副本。GC 允许客户端计算机和应用程序快速高效地查找和检索有关域中任何对象的详细信息，而无需联系每个域控制器。

#### 4、域控制器的分类

   **根据角色：**

- **主域控制器 (PDC)**：存储域的权威副本，处理写入操作和身份验证请求。

- **备份域控制器 (BDC)**：存储域的只读副本，提供冗余并处理只读请求。

- **只读域控制器 (RODC)**：存储域的只读副本，只处理不涉及写入操作的请求，增强了安全性和符合性。

#### 5、域的部署步骤

   **步骤：**

1. 安装域控制器：**在要充当域控制器的服务器上安装 Windows Server 操作系统。

1. 提升服务器：**使用“提升域功能”向导将服务器提升为域控制器。

1. 创建新林或加入现有林：**选择是否创建新 Active Directory 林或将服务器加入现有林。

1. 输入域名：**输入要创建或加入的域的名称。

1. 配置 DNS：**配置服务器上的 DNS 设置。

1. 创建林根域或子域：**选择是要创建林根域还是子域。

1. 输入域管理员凭据：**输入要用于管理新域的域管理员帐户的凭据。

1. 完成安装：**向导将完成域安装过程。

   **提升服务器为域控制器的步骤：**

1. 打开服务器管理器：**在 Windows Server 上，单击“开始”并搜索“服务器管理器”。

1. 选择“提升域功能”：**在“服务器管理器”的“工具”菜单中，选择“提升域功能”。

1. 选择提升选项：**在“提升域功能”向导中，选择是要创建新林还是将服务器加入现有林。

1. 输入域名：**输入要创建或加入的域的名称。

1. 输入 DNS 信息：**配置服务器上的 DNS 设置。

1. 选择林根域或子域：**选择是要创建林根域还是子域。

1. 输入域管理员凭据：**输入要用于管理新域的域管理员帐户的凭据。

1. 完成安装：**向导将完成域提升过程。

#### 6、添加额外的域控制器

为了提高冗余性和灾难恢复能力，您可能需要在您的网络中添加额外的域控制器。这可以通过运行“提升域功能”向导并选择“添加域控制器到现有域”选项来完成。完成这个过程后，新的域控制器将自动复制现有的域信息并开始接受请求。

其中如何利用介质来安装？输入以下命令即可，代码含义是在c盘根目录下创建一个123的备份镜像。

```
ntdsutil        # ntdsutil是用于管理和维护 Active Directory 的命令行工具
activate instance ntds    # 用于激活当前的 NTDS（Active Directory 数据库）实例
ifm      #代表Install From Media，这是 ntdsutil 中的一个子命令。
creat sysvol full c:\123  # 创建一个包含SYSVOL共享和Active Directory数据库的完整IFM媒体。这些文件将存储在 c:\123 目录中
```

#### 7、什么是子域

子域是父域下的一个或多个附属域，它继承了父域的一些设置和策略。子域可以用于划分网络资源，提高管理效率和安全性。

#### 8、域林

域林是 Windows Server 操作系统中的一种网络组织结构。它由一个或多个域树组成，每个域树都包含一个或多个域。域林中的所有域都共享一个全局编录，该编录包含域林中所有用户、计算机和其他对象的目录信息。

域林的优势在于它可以将大型网络划分为多个较小的域，从而简化管理。每个域都可以有自己的安全策略和管理人员。域林还可以提高网络的安全性，因为每个域都只能访问自己的资源。

**域林的组成**

域林由以下几个部分组成：

- **域**：域是域林的基本单位。它是一组共享安全策略和资源的计算机和用户集合。

- **域树**：域树是由一个或多个域组成的层次结构。域树中的所有域都具有相同的根域名。

- **域林**：域林是由一个或多个域树组成的层次结构。域林中的所有域都共享一个全局编录。

**域林的信任关系**

域林中的域之间存在信任关系。信任关系允许域相互验证用户和计算机的身份。域林中的信任关系可以是单向或双向的。