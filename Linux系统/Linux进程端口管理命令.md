### 1. 进程查看

**`ps`** = Process Snapshot（进程快照）

- `ps aux`：a=所有用户，u=显示详细信息，x=包含无终端进程

**`top`** = 实时刷新，像任务管理器(动态进程排行榜)

- 记忆：**进程在“顶部”动态跳动**

```
ps aux                     # 查看所有进程（最常用）
ps aux | grep代理名        # 比如：ps aux | grep v2ray
ps -ef | grep [p]roxy      # 另一种格式，[p]可避免grep自身匹配
top aux
```

##### 2.**`netstat -tulnp`** 拆解记忆：network网路+statistics状态/统计

- `t`=TCP，`u`=UDP（协议）
- `l`=listening（只显示监听的）
- `n`=no dns（不解析域名，显示数字端口）
- `p`=program（显示进程名/PID）

```
netstat -a #查看所有连接+监听端口
netstat -t #只看tcp连接的端口
```

#### 3.杀进程原理(kill只杀一个进程) killall可以杀多个

**`kill`** 本质是发信号给进程：

- `kill PID` = 发 SIGTERM(15) → 礼貌请进程自己退出

- `kill -9 PID` = 发 SIGKILL(9) → 强制且无法被进程忽略

- ##### **killall** PID/进程名字 #把所有和这个程序有关的进程全部杀掉

- ```
  kill 
  ```

  

```
kill PID
KIll -9 PID 
```

#### 4、systemctl 原理

**Systemd** 是系统管家，`systemctl` = system control

**命令结构记忆**：

text

```
systemctl + 动作 + 服务名
       动作：start/stop/status/restart/enable/disable
```



**易混点澄清**：

- `start` ≠ `enable`：start是现在开，enable是开机自动开
- `stop` ≠ `disable`：停掉 vs 禁止开机启动

**看状态**：`systemctl status 服务名` 会显示：active(跑着)/inactive(停了)/加载的配置文件/最近日志

### 5.lsof 超通俗解析（单词 + 作用 + 常用参数）

#### 一、单词拆解

**lsof = List Open Files**

直译：**列出所有已打开的文件**

Linux 万物皆文件：

普通文件、目录、端口、进程、设备、网络连接 **全算文件**

------

#### 二、核心功能

1. 查看**哪个进程打开了某个文件**

2. 查看**某个进程开了哪些文件 / 端口**

3. ## 查**端口被谁占用**（替代 netstat）

4. 删除文件还占空间，用 lsof 查谁还在占用

------

#### 三、最常用参数

### 1. lsof -i   (internet)

**查看所有网络连接、端口占用**

等同于看端口、TCP/UDP 连接

### 2. lsof -i : 端口

查**某个端口被哪个进程占用**



```
lsof -i :80
```

#### 3. lsof -p PID

看**某个进程 PID 打开了哪些文件**

#### 4. lsof 文件名

看**哪个进程正在占用这个文件**

#### 5. lsof -u 用户名

看**某个用户所有进程打开的文件**

## 

**lsof = List Open Files**

查文件、查端口、查进程占用，全能工具

#### 6.`ss` 是 Linux 里**比 netstat、lsof 更快、更现代**的查看端口 / 连接的工具，全称 **socket statistics**，专门看网络套接字。

```
ss -tuln | grep :22
```

拆开讲：

- `-t`：只看 **TCP** 连接（ssh 是 TCP）
- `-u`：只看 UDP
- `-l`：只看 **监听端口**（正在开放、等着别人连的端口）
- `-n`：**不解析域名 / 主机名**，直接显示 IP + 端口，更快

##### 7. 防火墙放行端口

```
sudo ufw disable #关闭防火墙
```

```
sudo ufw enable #开启防火墙
```

```
ufw allow 22/tcp #新增放行22端口规则
```

```
ufw delete allow 22/tcp #删除放行22端口规则
```

```
ufw deny 22/tcp #封死22端口，有放行规则也无法出入
```



