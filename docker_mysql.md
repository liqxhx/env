#

## 启动mysql容器
```shell
docker run --name mimesql -p 3306:3306  -e MYSQL_ROOT_PASSWORD=root -d mysql:5.5
```

## 切换到容器shell中
```shell
sudo docker exec -it mimesql bash
```
## 查看日志
```shell
docker logs mimesql
```

## 挂载数据
```shell
docker run --name mimesql -p 3306:3306 -v /Users/liqh/docker_project/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -d mysql:5.5

docker run --name mimesql -p 3306:3306 -v $PWD/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -d mysql:5.5

```
## 停止
```shell
docker stop fe && docker rm -v fe
```
## 连接
```shell
mycli  -hlocalhost -uroot -proot
```
## 创建数据库
```shell
 create database `test` default character set utf8 collate utf8_general_ci;
```
## 创建用户并授权
```
grant SELECT,INSERT,UPDATE,DELETE,CREATE,DROP on test.* to 'lqh'@'%' identified by 'lqh';
grant SELECT,INSERT,UPDATE,DELETE,CREATE,DROP on test.* to 'lqh'@'localhost' identified by 'lqh';
```
