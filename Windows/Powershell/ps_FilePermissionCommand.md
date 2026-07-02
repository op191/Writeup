Windows 权限管理核心总结
一、权限继承
概念	说明
继承	子文件/文件夹自动获得父级文件夹的权限
显式权限	直接分配在对象上的权限，不靠继承获得
查看时带 (I)	表示该权限是继承来的
查看时无 (I)	表示该权限是直接分配的

关键命令：
powershell

# 移除所有继承（只保留显式权限）
icacls 路径 /inheritance:r

# 禁用继承（保留当前继承权限转为显式）
icacls 路径 /inheritance:d

# 启用继承
icacls 路径 /inheritance:e

二、权限赋予和移除
操作	命令
授予权限	icacls 路径 /grant 用户:(权限)
移除用户所有权限	icacls 路径 /remove 用户
移除指定权限	icacls 路径 /deny 用户:(权限)
重置为默认权限	icacls 路径 /reset

权限代码速查：
代码	含义
F	完全控制
M	修改（读写删）
RX	读取和执行
R	只读
W	只写
三、查看权限
操作	命令
查看权限列表	icacls 路径
查看所有用户权限	icacls 路径 /t（含子目录）
四、操作逻辑图
text

权限来源
    │
    ├── 继承（从父级来）—— 可用 /inheritance:r 清除
    │
    └── 显式（直接分配）—— 可用 /grant、/remove 管理

查看时会显示：
    (I) = 继承来的
    无(I) = 显式分配的

五、常用实战组合
powershell

# 场景1：完全隔离一个文件夹（只让某用户访问）
icacls 文件夹 /inheritance:r          # 清空所有继承
icacls 文件夹 /grant 用户名:(F)        # 只给该用户完全控制

# 场景2：给用户添加权限（保留继承）
icacls 文件夹 /grant 用户名:(RX)       # 赋予只读执行权限

# 场景3：移除某个用户所有权限
icacls 文件夹 /remove 用户名

# 场景4：查看权限状态
icacls 文件夹                          # 快速查看

六、核心要点

        继承优先于显式 —— 子对象默认听父级的

        /inheritance:r 最常用 —— 需要独立控制权限时先用它清空继承

        /grant 追加，/remove 移除 —— 权限操作的本质是增删

        (I) 标记是关键 —— 看到它就说明权限来自上级

        管理员才能操作权限 —— 所有命令需以管理员身份运行 PowerShell

