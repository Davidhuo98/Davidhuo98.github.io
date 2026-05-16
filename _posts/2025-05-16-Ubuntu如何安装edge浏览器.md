---
layout: post
title: "Ubuntu如何安装edge浏览器"
date: 2025-05-16
tags: [Ubuntu]
---

正文内容写在这里...

# Ubuntu如何安装edge浏览器

在 Ubuntu 上安装 Microsoft Edge 浏览器的步骤相对简单，你可以通过官方的 Microsoft Edge 安用包来进行安装。以下是具体的安装步骤：

1. 下载 Microsoft Edge 安装包
首先，前往 Microsoft Edge 的官方下载安装页面：


或者你可以直接通过命令行下载 Microsoft Edge 的最新 .deb 安装包：

wget https://packages.microsoft.com/repos/edge/pool/main/m/microsoft-edge-stable/microsoft-edge-stable_<version>-1_amd64.deb
这里的 <version> 需要替换为实际的版本号，例如 112.0.1722.64，你可以从官方页面找到该信息。

2. 安装依赖
确保你的系统上有必要的依赖项（如 dpkg）：
sudo apt update
sudo apt install wget

3. 安装 Microsoft Edge
在下载完成之后，你可以使用以下命令来安装 .deb 包：


sudo dpkg -i microsoft-edge-stable_<version>-1_amd64.deb
如果出现缺少依赖的错误，你可以运行以下命令来修复缺失的依赖：


sudo apt --fix-broken install
这个命令会自动修复任何依赖问题，并确保安装过程完成。

4. 通过 APT 安装 Microsoft Edge（可选）
为了确保你始终能够获得 Microsoft Edge 的更新，可以将 Microsoft 的仓库添加到你的系统中。这可以通过以下步骤来完成：

添加 Microsoft Edge 的官方仓库

首先，确保你系统中有 wget 和 apt-transport-https，然后执行以下命令：

sudo apt install wget apt-transport-https

下载 Microsoft Edge 的 GPG 密钥：

wget -qO- https://packages.microsoft.com/keys/microsoft.asc | sudo tee /etc/apt/trusted.gpg.d/microsoft.asc
添加仓库地址

将 Microsoft Edge 的仓库地址添加到 /etc/apt/sources.list.d/ 目录下：


echo "deb [arch=amd64] https://packages.microsoft.com/repos/edge stable main" | sudo tee /etc/apt/sources.list.d/microsoft-edge.list
更新软件包列表并安装 Microsoft Edge

更新 APT 包索引并安装 Microsoft Edge：

sudo apt update
sudo apt install microsoft-edge-stable

5. 启动 Microsoft Edge
安装完成后，你可以通过应用程序菜单启动 Microsoft Edge，或者在终端中运行：

microsoft-edge