功能	PowerShell 命令	CMD 老命令	备注
看用户	Get-LocalUser	net user	输出对象，可过滤
看组	Get-LocalGroup	net localgroup	同样可管道操作
看组成员	Get-LocalGroupMember Administrators	net localgroup administrators	直接看管理员组成员
看进程	Get-Process	tasklist	别名 ps（和Linux一样！）
看服务	Get-Service	sc query	别名 gsv
看网络连接	Get-NetTCPConnection	netstat -ano	更结构化，可过滤状态
看文件/目录	Get-ChildItem	dir	别名 ls（和Linux一样！）
看内容	Get-Content	type	别名 cat（和Linux一样！）
看当前用户	WhoAmI	whoami	命令基本一样
看系统信息	Get-ComputerInfo	systeminfo	输出结构化对象

重点：Get-Process 可以直接用 ps，Get-ChildItem 可以用 ls，Get-Content 可以用 cat——和 Linux 习惯无缝衔接。
 

 用户管理（需管理员权限）
powershell

# 创建本地用户
New-LocalUser 然后他会让你输入账户和密码

# 查看所有本地用户
Get-LocalUser

# 查看特定用户信息
Get-LocalUser -Name "用户名"

# 删除用户
Remove-LocalUser -Name "用户名"

📁 组管理
powershell

# 查看所有本地组
Get-LocalGroup

# 查看组内成员
Get-LocalGroupMember -Group "Administrators"

# 将用户添加到组（常用）
Add-LocalGroupMember -Group "组名" -Member "用户名"

# 从组移除用户
Remove-LocalGroupMember -Group "组名" -Member "用户名"

常用内置组：Administrators、Users、Guests、Power Users
📄 文件操作
powershell

# 创建空文件
New-Item -Path "C:\路径\文件名.txt" -ItemType File

#移除文件
rm 路径

# 创建带内容的文件
Set-Content -Path "C:\路径\文件名.txt" -Value "第一行内容"

# 追加内容到文件
Add-Content -Path "C:\路径\文件名.txt" -Value "追加的新行"（echo 'fjies' >> pp.txt  也可以）

# 创建文件夹
New-Item -Path "C:\路径\新文件夹" -ItemType Directory（mkdir 路径 也可以）

# 查看文件/文件夹列表
Get-ChildItem -Path "C:\路径"（ls 路径 也可以）

# 查看文件内容
Get-Content -Path "C:\路径\文件名.txt"（cat 路径 也可以）
