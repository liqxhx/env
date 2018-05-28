#

```
启动mysql容器
docker run --name mimesql -p 3306:3306  -e MYSQL_ROOT_PASSWORD=root -d mysql:5.5

用mycli连接到mysql容器
mycli  -hlocalhost -uroot -proot

切换到容器shell中
sudo docker exec -it mimesql bash

查看日志
docker logs mimesql

挂载数据
docker run --name mimesql -p 3306:3306 -v /Users/liqh/docker_project/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -d mysql:5.5

docker stop fe && docker rm -v fe

```
