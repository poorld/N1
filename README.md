# n1优秀帖子集合
目前使用稳定
[Armbian 5.77 debian+docker+Portainer汉化面板+openwrt的DDBR备份](https://www.right.com.cn/forum/thread-3511151-1-1.html)

[只要你的N1能U盘启动，N1系统随心换，DDBR备份的各](https://www.right.com.cn/forum/thread-4043095-1-1.html)

[启用docker openwrt后 armbian宿主机的上网问题解决方案](https://www.right.com.cn/forum/thread-1066223-1-1.html)

[40多个N1可用docker镜像-百度云同步、1898种dos游戏、花生壳、可道云、蚂蚁笔记](https://www.right.com.cn/forum/thread-911375-1-1.html)



# docker安装记录

## nginx 8082
--------------------------------------
```
docker run -d -p 8082:80 --name docker-nginx \
-v /data/nginx/www:/usr/share/nginx/html:ro \
-v /data/nginx/conf/nginx.conf:/etc/nginx/nginx.conf \
-v /data/nginx/logs:/var/log/nginx \
-v /mnts/download:/usr/share/nginx/download:ro \
nginx
```

## aria2NG
--------------------------------------
```
docker run --name aria2-ariang \
--restart always \
-p 6800:6800 -p 6880:80 -p 6888:8080 \
-v /mnts/download:/aria2/downloads \
-v /data/aria2/conf:/aria2/conf \
-e SECRET=086a0519b7556e9dbc93 \
colinwjd/aria2-ariang
```

## oneindex
--------------------------------------
这个没用
```
docker run -d --name oneindex \
    -p 8083:80 --restart=always \
    -v /data/oneindex/config:/var/www/html/config:ro \
    -v /data/oneindex/cache:/var/www/html/cache:rw \
    -e REFRESH_TOKEN='0 * * * *' \
    -e REFRESH_CACHE='*/10 * * * *' \
    setzero/oneindex
```

这个可以
```
docker run -d \
  --name oneindex \
  -p 8086:80 \
  --restart=always \
  -v /data/oneindex/config:/var/www/html/config \
  -v /data/oneindex/cache:/var/www/html/cache \
  -e REFRESH_TOKEN='0 * * * *' \
  -e REFRESH_CACHE='*/10 * * * *' \
  lstcml/oneindex
```

## bypy
--------------------------------------
```
docker run -dit \
  --name bypy \
  --restart=always \
  -v /docker/bypy/sync:/sync \
  lstcml/bypy
```

## markdown
--------------------------------------
```
docker run -d \
  --name markdown \
  --restart=always \
  -p 8087:2000 \
  lstcml/n1_markdown
```

## 安装并配置Samba
--------------------------------------
```
docker pull dperson/samba

docker run -d \
  --restart=always \
  --name samba -p 139:139 \
  -p 445:445 --hostname 'N1' -v \
  /mnts/download:/mount -d dperson/samba \
  -u "username;password"  -s "FileShare;/mount;yes;no;yes;all;none" \
  dperson/samba
```

## tomact(Apache Tomcat/9.0.37，jdk8)
--------------------------------------
```
docker pull tomcat:jdk8-adoptopenjdk-hotspot
```

```
docker run  -d \
--restart=always \
--name tomcat \
-p 8080:8080 \
-v /data/tomcat/webapps/examples:/usr/local/tomcat/webapps/examples \
tomcat:jdk8-adoptopenjdk-hotspot
```
输入http://192.168.1.10:8080/examples即可看到效果


## mysql(只有8.0才支持arm64/v8)
--------------------------------------
```
docker pull mysql/mysql-server:latest

docker run -d \
-p 3306:3306 \
--name mysql \
--restart=always \
mysql/mysql-server
```
查看密码
```
docker logs mysql 2>&1 | grep GENERATED
```
进入mysql
```
docker exec -it mysql mysql -uroot -p
```
