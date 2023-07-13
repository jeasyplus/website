# MySQL慢SQL

## 开启慢SQL日志
```roomsql
-- my.ini或 my.cnf
slow_query_log = 1
slow_query_log_file = /var/log/slow.log
long_query_time = 1
```
配置后重启


