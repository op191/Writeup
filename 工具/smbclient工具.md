- **-L**：`List`，列出服务器的所有共享资源（不登录查看）
- **-U**：`User`，指定用户名（如 `-U bob` 或 `-U domain/bob`）
- **-N**：`No password`，空密码登录
- **-p**：`Port`，指定 SMB 端口（默认 445 或 139）
- **-I**：`IP address`，直接指定目标 IP（绕过 NetBIOS 名称解析）
- **-W**：`Workgroup`，指定工作组/域
- **-c**：`Command`，执行一条命令后退出（如 `-c 'ls'`）
- **-m**：`Maximum protocol`，指定最高 SMB 协议版本（如 `-m SMB3`）
- **-d**：`Debug level`，调试级别（0-10）
- **-A**：`Auth file`，使用认证文件（每行 username=、password= 等）
- **-O**：`Socket options`，套接字选项（很少用）

smbclient -L //目标 其他参数