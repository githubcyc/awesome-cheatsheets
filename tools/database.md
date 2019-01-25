
## MySQL

```
mysql -u root -p 123 -h 127.0.0.1 -P 3306
> show databases;
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
> use test
> db.foo.find({})
> db.foo.deleteMany({})
```