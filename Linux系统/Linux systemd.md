# Linux systemd

## 一、核心总纲

1. **systemd**：Linux 系统一号管家，PID=1，系统第一个启动进程
2. **万物皆 Unit**：systemd 管理的**所有对象 = Unit**，**后缀区分对象类型**
3. 统一命令：所有 Unit 全部用`systemctl`一套命令管理

## 二、五大核心 Unit（后缀 = 对象类型，必背）

表格

| 后缀类型       | 作用本质     | 通俗解释                           |
| ---------- | -------- | ------------------------------ |
| `.service` | 应用服务单元   | 后台常驻程序（Nginx、SSH、网络服务）         |
| `.target`  | 系统状态标记单元 | **无进程、不运行程序**，只是开机节点 / 分组、启动标尺 |
| `.mount`   | 磁盘挂载单元   | 管理硬盘、分区、U 盘开机自动挂载              |
| `.timer`   | 定时任务单元   | 替代 crontab，定时执行服务              |
| `.network` | 网卡配置文件   | 给网卡配置 IP、网关，**不是服务**           |

### 关键规则

1. `systemctl start xxx`：**默认省略.service**
2. `.mount/.timer/.target/.network` 后缀**必须写全**
3. 修改 Unit 配置，必须执行：`systemctl daemon-reload`

## 三、.target 深度总结（你最疑惑点）

1. 本质：**虚拟标记、状态打卡点，没有进程、不占资源**

2. `After=xxx.target`：**启动顺序，等这个系统状态完成，再启动当前服务**

3. 两个高频 target
- `network.target`：网络服务已启动 ≠ 网卡有 IP、不能上网

- `network-online.target`：**网卡获取 IP、网络彻底连通**
  
  ✅ 网页 / 后端 / 联网服务**必须写 After=network-online.target**
4. `WantedBy=multi-user.target`：服务器命令行模式，开机自启绑定入口

## 四、U 盘挂载核心答疑

1. 桌面 Linux：自带`udisks2`进程 → 插 U 盘**自动挂载**
2. 服务器 Linux：无桌面、无自动挂载进程 → 只能手动挂载
3. `lsblk`：查看 U 盘硬件设备名`sdb1`，找到设备才能挂载
4. `.mount单元局限`：只能挂载**写死的固定 U 盘**，新插入陌生 U 盘识别不了

## 五、进程核心笔记

1. 进程：**跑起来的程序**，每个进程有唯一 PID
2. 前台进程：依附终端，关 SSH / 按 Ctrl+C 直接杀死
3. 后台守护进程：脱离终端，24 小时运行，服务专用
4. systemd 作用：托管后台进程，崩溃自动重启、开机自启

## 六、网络配置笔记

1. 网卡名称：ens33、eth0，`ip a`查看 IP 与网卡状态

2. 两套网络管理（二选一冲突）
   
   ① NetworkManager：桌面 / 虚拟机，命令`nmcli`
   
   ② systemd-networkd：systemd 原生服务，纯服务器使用

3. `systemctl start systemd-networkd`：启动**网络服务进程**

4. `/etc/systemd/network/*.network`：网卡 IP 配置文件，纯配置、不能 start

## 七、日志系统 journald

1. 归属：`systemd-journald.service` systemd 自带日志服务
2. 特点：二进制日志、统一收集所有服务日志，不用读零散文本
3. 核心命令（排错必用）

bash

运行

```
journalctl -u 服务名   # 查看单个服务日志
journalctl -u 服务名 -f # 实时刷新日志
journalctl -b  # 本次开机全部日志
```

4. 服务报错优先看：`systemctl status 服务名` + 对应 journal 日志

## 八、服务部署标准流程（企业通用）

把普通程序 → 系统后台服务，5 步标准流程

1. 写自定义 `.service` 文件（/etc/systemd/system/）
2. 配置：启动命令、网络依赖、自动重启、开机绑定 target
3. `systemctl daemon-reload` 重载配置
4. `systemctl enable --now 服务名` 启动 + 开机自启
5. status 查看状态、journal 排查报错

### 标准 service 模板

ini

```
[Unit]
Description=业务服务
After=network-online.target # 网络就绪再启动
Wants=network-online.target

[Service]
ExecStart=程序绝对路径
Restart=on-failure # 崩溃自动重启

[Install]
WantedBy=multi-user.target # 开机自启
```

## 九、虚拟化极简总结

1. 虚拟化：软件拆分物理硬件，模拟独立电脑 / 环境

2. 两类虚拟化
- 虚拟机 (KVM/VMware)：模拟完整硬件，**内部自带独立 systemd、整套系统**，资源大、隔离强

- 容器 (Docker)：共享宿主机 Linux 内核，轻量、无完整系统、无 systemd
3. 虚拟化服务：libvirtd、docker 全部是`.service`，由 systemd 统一管理

## 十、今日整条逻辑闭环（终极串联）

物理机开机 → 内核启动 → **systemd(PID=1)**启动

→ 管控所有 Unit：

网络服务 + 磁盘挂载 + 系统状态 target + 自定义业务服务

→ 拉起所有后台进程、网卡配置、硬件挂载

→ 日志统一收集

→ 虚拟化服务启动，运行虚拟机 / 容器

→ 虚拟机内部：复刻一模一样的 systemd 整套体系
