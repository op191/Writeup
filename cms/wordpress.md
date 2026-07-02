wpscan 是个漏洞扫描工具，专门对cms为wordpress制作的，可以枚举用户

用法：

wpscan --url http://192.168.168.15 --enumerate u 

也可以枚举插件/主题，因为插件主题可以做上传恶意脚本用；

用法：

wpscan --url http://192.168.185.15 --enumerarte p   

p 主题  t 插件  vt,vp 主题和插件+他们漏洞同时枚举

基础扫描：

wpscan --url http://192.168.158.3   可以看版本

wpscan 也可以用来做暴力破解：
wpscna --url http://1192.168.18.1  -U filename.txt -P filename.txt   一定用大写