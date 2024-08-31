---
layout:       post
title:        "Github Pages 如何添加自定义域"
author:       "Gweek"
header-style: text
catalog:      true
tags:
    - github
    - 自定义域名    
    
---
## 1. 购买和设置域名
首先，你需要一个你拥有的域名。你可以通过任何域名注册服务商（如GoDaddy, Namecheap等）购买域名。

## 2. 配置DNS记录
在你的域名注册商的管理面板中，你需要添加一些DNS记录以指向GitHub的服务器。

- A记录

如果你使用的是顶级域名（例如 example.com），你需要添加以下A记录：

记录类型：A

主机名：@ 或留空（取决于你的域名注册商）

值：185.199.108.153

值：185.199.109.153

值：185.199.110.153

值：185.199.111.153

- CNAME记录

如果你使用的是子域名（例如 www.example.com），你需要添加以下CNAME记录：

记录类型：CNAME

主机名：你的子域名（例如 www）

值：你的 GitHub Pages 的地址，例如 username.github.io

## 3. 在GitHub仓库中配置
在GitHub仓库的设置中配置自定义域名。

- 进入你的 GitHub 仓库。

- 点击页面右上角的“Settings”（设置）按钮。

- 在“Code and automation”下的“Pages”部分，找到“Custom domain”选项。

- 在“Custom domain”字段中输入你的自定义域名（例如 www.example.com 或 example.com）。

- 点击“Save”按钮保存。

## 4. 配置CNAME文件
为了确保你的自定义域名正确地绑定到你的GitHub Pages站点，你需要在你的仓库根目录下创建或更新一个名为CNAME的文件，文件内容为你的自定义域名。

例如：

`www.example.com`

## 5. 等待DNS生效
DNS更改可能需要一些时间才能生效，通常在24小时内。

## 6. 验证设置
最后，访问你配置的自定义域名，确认它是否正确指向你的GitHub Pages站点。如果一切正常，你的自定义域名应该可以正常访问你的GitHub Pages。

通过以上步骤，你就可以成功地为你的GitHub Pages站点添加一个自定义域名了。
