---
title: "Linux Notes"
date: 2026-05-11T10:33:58+08:00
draft: false
tags: [Linux]
categories: []
description: ""
---

记录Linux笔记，系统版本为Ubuntu-24.04-LTS。

## 一、常用

### 1. 文件备份加时间戳

```bash
文件名.bak.$(date +%Y%m%d%H%M%S)
```

## 二、系统优化

### 1. 删除终端启动文字

```bash
# 方法一：
# 原理：每次打开终端，自动执行 clear 命令，把欢迎信息清掉。

# 打开终端，执行以下命令编辑配置文件
nano ~/.bashrc

# 用方向键滚动到文件的最后一行，添加：
clear

# 配置立即生效
source ~/.bashrc


# 方法二：
# 把 clear 追加到 .bashrc 末尾
echo "clear" >> ~/.bashrc

# 让配置立即生效
source ~/.bashrc
```

### 2. 修改主源

修改sources.list.d/ubuntu.sources中的URIs即可

```bash
Types: deb
URIs: http://mirrors.aliyun.com/ubuntu/
Suites: noble noble-updates noble-backports
Components: main universe restricted multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

Types: deb
URIs: http://mirrors.aliyun.com/ubuntu/
Suites: noble-security
Components: main universe restricted multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
```

### 3. 增加字体

```bash
# 新建文件夹
mkdir -p ~/.local/share/fonts

# 给字体设置权限（可选但推荐），确保所有用户都能读取这些字体文件
chmod 644 ~/.local/share/fonts/*

# 放入字体后刷新缓存
fc-cache -fv

# 验证是否成功，用下面的命令，看看系统能不能找到 “宋体（SimSun）”：
fc-list :lang=zh | grep -i simsun

# 重启软件即可看到新字体
```

补充：为什么不直接在根目录建？
- 现在的路径是 /home/用户名/.local/share/，属于当前用户私有目录
- 在这里创建的 fonts 文件夹，只有当前用户能用，不会影响其他用户，也不需要管理员权限，是最安全的方式。

### 4. 下载nvm

```bash
# 从国内镜像克隆源码
git clone --branch v0.40.1 https://gitee.com/mirrors/nvm.git ~/.nvm && \

# 设置变量路径
echo 'export NVM_DIR="$HOME/.nvm"' >> ~/.bashrc && \

# 注入脚本启动逻辑（核心）
echo '[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"' >> ~/.bashrc && \
echo '[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"' >> ~/.bashrc && \

# 立即激活当前环境
source ~/.bashrc
```

加速下载 Node

```bash
# 将阿里云镜像地址写入配置文件
echo 'export NVM_NODEJS_ORG_MIRROR=https://npmmirror.com/mirrors/node' >> ~/.bashrc

# 刷新配置使之立即生效
source ~/.bashrc
```

设置npm下载镜像

```bash
npm config set registry https://registry.npmmirror.com
```

## 三、软件安装

### 1. Git 安装

```bash
# Git 本身必须安装到全局系统
sudo apt update
sudo apt install -y git

# 看版本
git --version
```

### 2. VSCode 安装

```bash
# 安装必要工具
sudo apt update
sudo apt install -y wget gpg apt-transport-https software-properties-common

# 导入微软签名密钥
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -D -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/packages.microsoft.gpg

# 添加 VSCode 官方源
echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/trusted.gpg.d/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" | sudo tee /etc/apt/sources.list.d/vscode.list > /dev/null

# 安装 VSCode
sudo apt update
sudo apt install -y code
```

### 3. Chrome 安装

```bash
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo apt install ./google-chrome-stable_current_amd64.deb
```

### 4. 网易云安装

```bash
# 安装Flatpak
sudo apt install flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

# 安装GTK4版
flatpak install flathub com.github.gmg137.netease-cloud-music-gtk

# 运行
flatpak run com.github.gmg137.netease-cloud-music-gtk
```

### 5. RustDesk 安装

```bash
wget https://github.com/rustdesk/rustdesk/releases/download/1.2.3/rustdesk-1.2.3-x86_64.deb
sudo dpkg -i rustdesk-1.2.3-x86_64.deb
sudo apt install -f
```