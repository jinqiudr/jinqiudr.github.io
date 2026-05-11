---
title: "Linux Notes"
date: 2026-05-11T10:33:58+08:00
draft: false
tags: [Linux]
categories: []
description: ""
---

记录Linux笔记，系统版本为Ubuntu-24.04-LTS。

### 一、常用

#### 1. 文件备份加时间戳

```bash
文件名.bak.$(date +%Y%m%d%H%M%S)
```

### 二、软件安装

#### 1. git安装

```bash
# Git 本身必须安装到全局系统
sudo apt update
sudo apt install -y git

# 看版本
git --version
```

#### 2. vscode安装

```bash
# 1. 安装必要工具
sudo apt update
sudo apt install -y wget gpg apt-transport-https software-properties-common


# 2. 导入微软签名密钥
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -D -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/packages.microsoft.gpg


# 3. 添加 VSCode 官方源
echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/trusted.gpg.d/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" | sudo tee /etc/apt/sources.list.d/vscode.list > /dev/null


# 4. 安装 VSCode
sudo apt update
sudo apt install -y code
```

