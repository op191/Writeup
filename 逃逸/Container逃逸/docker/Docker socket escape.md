```
你（在容器A里）
    │
    │  docker run -v /:/host ubuntu bash
    │
    ▼
/var/run/docker.sock ──► 宿主机的 dockerd
                              │
                              │  dockerd 收到指令，自己去创建容器B
                              │  dockerd 在宿主机上，所以 / 就是宿主机的 /
                              ▼
                         容器B（挂载了宿主机的 /）
```

你在容器A里敲的命令，实际上是通过 socket 发给宿主机的 dockerd，dockerd 是跑在宿主机上的进程，它理解的 `/` 永远是宿主机的根目录。

对比一下正常情况：

```
正常容器                    socket挂载的容器

docker CLI                  docker CLI
    │                           │
    │ 无法访问socket             │ 通过挂载的socket
    │                           ▼
    ✗                      宿主机 dockerd
                                │
                                ▼
                           宿主机视角执行命令
```

所以本质上你根本没有"从容器A操作容器A的文件系统"，你是**借用容器A的身份，让宿主机帮你开了容器B**，宿主机不知道也不在乎你是从容器里发来的指令还是直接在宿主机上发的。

这就是为什么这个配置错误这么危险——拿到 socket 等于拿到宿主机 dockerd 的完整控制权。
挂载宿主机的/到容器的/host目录后 直接
```
chroot /host bash
```

chroot:`change root`就是改变根目录，然后bash打开一个shell