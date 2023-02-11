---
title: nvm环境安装,node环境安装
date: 2023-02-11 15:15:31
categories:
- [日常工作经验记录,前端,搭建个人博客,环境准备]
  tags:
- 博客
- hexo
---


# 安装脚手架
```shell
npm install hexo-cli -g
```

# 初始化
```shell
mkdir /data/app/myblog
cd /data/app/myblog
hexo init
```

# 常用命令
```shell
# 清理缓存
hexo clean
# 构建
hexo g
# 启动本地服务
hexo s
# 部署
hexo d 
```


# 其他报错
## Error: ENOENT: no such file or directory, uv_cwd
用的nvm管理nodejs,查询资料说需要重启终端.试了试。可以将目标版本设定为默认版本
退出重新登录用户.
重新安装hexo脚手架
```shell
nvm alias default 12.15.0
# 退出用户 重新登录后
npm install hexo-cli -g
```