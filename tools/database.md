
## MySQL

```
mysql -u root -p 123 -h 127.0.0.1 -P 3306
> show databases;
```

###  migration

* method 1

```
// use file
> select * from fans into outfile '/data/fans.txt';

// copy
zip fans.zip /data/fans.txt
scp fans.zip root@ip:/data/ 

// unzip
unzip /data/fans.zip
> load data infile '/data/fans.txt' into table new_fans(id, appid, openid,unionid, @dummy, created_at, @dummy);

// 通过@dummy丢弃掉不需要的目标字段数据
```

* mysql安全项设置
>  在mysql执行load data infile和into outfile命令都需要在mysql开启了secure_file_priv选项，可以通过show global variables like '%secure%';查看mysql是否开启了此选项，默认值Null标识不允许执行导入导出命令。通过vim /etc/my.cnf修改mysql配置项，将secure_file_priv的值设置为空：

```
[mysqld]
secure_file_priv=''
```

* method 2

```
// 1. dump
mysqldump -h 127.0.0.1 -u user_name -p --opt
--default-character-set=utf8 --extended-insert=false --triggers
--skip-triggers
--hex-blob db_name > /tmp/db_name.sql

// 其中user_name以您真实的数据库用户代替 db_name以您真实的数据库名代替。 /tmp/db_name.sql是文件名，由您自己随意填写。

// 2. 将数据文件使用ftp工具上传到已经购买的云服务器中。
// 3. 远程登录到云服务器，将刚才上传的数据文件导入到172.0.0.2:3306中。例如：
mysql -h 172.0.0.2 -u user_name -p db_name <
/tmp/db_name.sql
```

## Redis

```
redis-cli -h <host>
> auth "psw"
```

## mongo

```
mongo host:port
> use admin
> db.auth('root', 'psw')
> show dbs
> use test
> db.foo.find({})
> db.foo.deleteMany({})
```

* mongo data migration
* https://help.aliyun.com/document_detail/60157.html

```
cd ~/data
// backup
mongodump --host 127.0.0.1 --port 27017 -u root --authenticationDatabase admin -d <dbname>

// restore
mongorestore --host 127.0.0.1:27017 -u root --authenticationDatabase admin dump
```


## Refer

* [Mysql data migration](https://blog.csdn.net/qq_41790443/article/details/80885287)
* :star:[MySQL-大批量数据如何快速的数据迁移](https://blog.csdn.net/qq_26334813/article/details/80503973)