---
layout:       post
title:        "Google云设置SSH登陆"
author:       "Gweek"
header-style: text
catalog:      true
keywords:     谷歌云 SSH
tags:
    - Google云 
    - SSH登陆


---

### 一、修改ssh配置文件并设置root密码

1.首先使用Google Cloud SSH登录VPS

2.切换到root账户 `sudo -i`

3.编辑sshd配置文件 `vim /etc/ssh/sshd_config`

4.修改以下内容即可 按键盘【i】进入编辑，按【Esc】退出编辑，再输入 :wq 保存并退出

![f4a69e069cdffd536fc83.png](https://img.myla.eu.org/file/f4a69e069cdffd536fc83.png)

![2c0b69d11072065e3f8d3.png](https://img.myla.eu.org/file/2c0b69d11072065e3f8d3.png)

5.重启sshd服务 `service sshd restart`

6.为root账户设置密码 `passwd`

输入密码 输入密码的时候不会显示出来，所以直接输入密码，然后回车，再然后重复输入密码回车

确认密码 再输入一次

设置成功
