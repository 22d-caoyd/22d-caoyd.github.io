---
title: "ssh-diier-with https"
date: 2025-11-28
tags: ["ssh","https", "网络协议"]
categories: ["今日所学","计算机基础"]
draft: false
---

### 1. 安全性

- SSH（Secure Shell） 是一种安全的加密协议，用于安全地连接远程服务器，通常用于 Git 操作（如 git push, git pull）时认证身份。SSH 使用密钥对（私钥和公钥）进行身份验证，每次提交时无需密码。

    - 优势：更安全，不需要每次输入用户名和密码，防止中间人攻击。

 - HTTPS（Hypertext Transfer Protocol Secure） 是基于 HTTP 的加密协议，用于 网络通信，通常用于 Git 操作（如 git push, git pull）时传输数据。HTTPS 使用用户名和密码或 token 进行身份验证。

    - 优势：广泛支持，大多数情况下都可以使用，只要你有用户名和密码（或者 GitHub Token）即可。

### 2. 使用方式和配置

SSH：

你需要在本地生成 SSH 密钥对（私钥和公钥），并将公钥添加到 GitHub 等远程仓库。

使用 SSH 时，不需要每次输入用户名和密码，SSH 密钥验证通过后可以直接进行操作。

```bash
生成 SSH 密钥对：

ssh-keygen -t rsa -b 4096 -C "你的GitHub邮箱"

```
将公钥 id_rsa.pub 上传到 GitHub 设置中的 SSH 密钥部分。

HTTPS：

你直接使用 用户名和密码 或 GitHub Token 来进行身份验证。

在推送代码时，你需要输入用户名和密码，或者配置 GitHub Token 来代替密码，避免每次都输入密码。
```bash

典型的 HTTPS URL：

https://github.com/username/repository.git

```
- URL 一般由以下几部分组成：
```ruby
协议://域名/路径/文件名
```


使用 git push 或 git pull 时，Git 会要求你输入用户名和密码（或者 GitHub Token）。

### 3. 认证方式：

SSH 认证：

使用 私钥和公钥 配对验证身份。每次推送时，无需再次输入用户名和密码。

如果没有设置 passphrase，SSH 不会要求输入任何密码。

HTTPS 认证：

使用 用户名和密码 或 GitHub Token 来验证身份。

每次推送时，需要输入用户名和密码（或者 GitHub Token，密码的替代品），或者将凭证保存起来以避免每次输入。


### 4. 网络连接：

SSH 需要通过 22 端口连接，因此在某些网络环境下可能会受到防火墙或网络限制，无法连接。

HTTPS 使用的是 443 端口（与网页浏览器一样），大部分网络环境都不会封锁 HTTPS，因此可以通过浏览器或 Git 客户端更稳定地连接。

### 5.其他
- 端口：每个端口负责不同的服务或协议
  - 如github的22端口处理ssh的交互
- API：服务接口，用于让用户和系统进行交互
  - eg：openai的服务器提供的gpt api服务

