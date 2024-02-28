使用homebrew直接安装：
```shell
brew install nginx
```
启动(安装完后应该已经启动了)：
```shell
sudo nginx
```
直接访问`localhost:80`即可，出现了nginx欢迎页面

# 位置
---
- 安装位置：`/opt/homebrew/bin/nginx`
- 前端资源目录：`/opt/homebrew/var/www`，一般将前端页面文件夹放在这，配置文件中就可以写从www文件夹下开始的相对路径了
- 默认的配置文件在：`/opt/homebrew/etc/nginx/nginx.conf`
**一般在配置文件中会指定前端页面位置，并且进行相关网络配置。运行时指定或使用默认配置文件！！！**