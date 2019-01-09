# MySQL

Includes the following services:
- Load Balancer
- MySQL Server
- Adminer

## 说明

```shell
## docker ps 状态由 (health: starting) 变成 (healthy)
## 查看root密码
sudo docker logs mysql1 2>&1 | grep GENERATED

## 进入mysql
sudo docker exec -it mysql1 mysql -uroot -p
## GENERATED ROOT PASSWORD: Axegh3kAJyDLaRuBemecis&EShOs

## 修改root可远程访问
SET PASSWORD = PASSWORD('abcd1234');
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'abcd1234' WITH GRANT OPTION;
flush privileges;

## 主
GRANT REPLICATION SLAVE ON *.* to 'reader'@'%' identified by 'abcd1234';
flush privileges;
show master status;

## 从
grant SHOW DATABASES,SELECT on *.* to 'slave'@'%' identified by 'abcd1234';
flush privileges;

stop slave;
change master to master_host='10.168.12.204',master_port=3316,master_user='reader',master_password='abcd1234',master_log_file='mysql-bin.000003',master_log_pos=216340;
start slave;
## 查看集群状态
show slave status\G
```
