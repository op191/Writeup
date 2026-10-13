**权限/用户管理相关**

- **`\pipe\samr`** —— SAM Remote 协议，枚举域用户/组/计算机账号、查权限归属。`nxc --users`、`samrdump.py`、RID cycling 全靠它，之前你被 `RestrictRemoteSAM` 卡住的那次走的就是这个管道。
- **`\pipe\lsarpc`** —— LSA（本地安全机构）接口，查 SID↔用户名互转（`lookupsid.py` 就是靠这个批量猜 SID 爆破用户名）、查域内安全策略、LSA secrets 相关操作。

**服务控制/远程执行相关**

- **`\pipe\svcctl`** —— 服务控制管理器（SCM）接口。`psexec.py`、`smbexec.py` 这类靠"创建一个 Windows 服务来执行命令"的工具，走的就是这个管道去 `CreateService`/`StartService`/`DeleteService`。
- **`\pipe\atsvc`** —— 任务计划（Task Scheduler/AT 服务）接口。`atexec.py` 靠它远程建一个计划任务、立刻触发执行、再删掉，拿到命令输出。
- **`\pipe\winreg`** —— 远程注册表服务接口。`secretsdump.py` 读 SAM/SECURITY hive、`reg.exe` 远程操作、还有你刚才用的nxc的 `--sam`/`--lsa` 模块，都是靠这个管道读注册表（这也是为什么之前 `secretsdump` 能绕开 Defender 拦截输出文件那个坑——它走的是读注册表，不是"写文件再读"那套）。

**DCOM/WMI 相关**

- **`\pipe\epmapper`** —— RPC Endpoint Mapper 的命名管道版（对应裸 RPC 里 135 端口那个 endpoint mapper 的功能），`wmiexec.py`/`dcomexec.py` 走 DCOM 的时候，内部协商具体接口在哪个端口/管道上，会先问它。

**域信任/Kerberos相关**

- **`\pipe\netlogon`** —— Netlogon 服务接口，域间信任通道建立、机器账号认证走这里。**Zerologon（CVE-2020-1472）** 这个经典洞打的就是这个管道——利用 Netlogon 协议里 AES-CFB8 加密模式的缺陷，把域控的机器账号密码直接置空，拿到域管权限。

**打印相关（横向移动常客）**

- **`\pipe\spoolss`** —— 打印后台处理服务（Print Spooler）接口。**PrinterBug/SpoolSample** 和 **PrintNightmare（CVE-2021-1675/CVE-2021-34527）** 都是打这个管道——前者是强制域控对你发起认证（经典的 coercion 技术，你之前用 `coercer.py` 打 braavos 用的也是类似思路），后者是直接拿来提权/远程执行代码。

**文件服务器相关**

- **`\pipe\srvsvc`** —— 服务器服务接口，枚举共享（`net view`/`nxc --shares` 底层很多时候走这个）、查服务器基本信息。

**快速对照表记忆法：**

| 管道       | 对应功能       | 典型工具                       |
| -------- | ---------- | -------------------------- |
| samr     | 用户/组枚举     | nxc --users, samrdump.py   |
| lsarpc   | SID查询/LSA  | lookupsid.py, --lsa        |
| svcctl   | 服务控制(执行命令) | psexec.py, smbexec.py      |
| atsvc    | 计划任务(执行命令) | atexec.py                  |
| winreg   | 远程注册表      | secretsdump.py, --sam      |
| netlogon | 域信任/机器认证   | zerologon                  |
| spoolss  | 打印服务       | printerbug, printnightmare |
| srvsvc   | 共享枚举       | --shares                   |

想深挖某一个的话（比如 Zerologon 具体怎么打、或者 `svcctl` 这条路为什么比 `atsvc` 更容易被 AV 盯上）


