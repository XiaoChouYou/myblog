---
title: 1-nvm环境安装,node环境安装
date: 2023-02-11 15:15:31
categories:
  - [日常工作经验记录,前端,搭建个人博客,环境准备]
tags:
  - nvm
  - node
---


# 下载并安装nvm
```shell
mkdir /data/nodejs_pkg/
cd /data/nodejs_pkg/
git clone https://github.com/nvm-sh/nvm.git ~/.nvm
echo "source ~/nvm/nvm.sh" >> ~/.bashrc
source ~/.bashrc
```

# 查看可安装的版本
```shell
nvm ls-remote
```

# 下载并安装nodejs
```shell
nvm install 12.15.0
nvm alias default 12.15.0
```
* 高版本nodejs需要的内核版本较高,根据自己实际情况选择对应版本

# 修改国内镜像源
```shell

npm config set registry         https://registry.npm.taobao.org
npm config set sass_binary_site https://npm.taobao.org/mirrors/node-sass/
npm config set disturl https://npm.taobao.org/dist --global

npm install --global yarn
yarn --version
```


