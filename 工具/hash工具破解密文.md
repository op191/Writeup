先echo 密文 filename.txt 把密文写入一个文本

然后hash -m  加密模式ID 密文文件 字典路径

如 

hash -m **3200** pp.txt **/usr/share/wordlists/rockyou.txt**

-m mode 表示模式 只能在后面写加密模式的ID ,如果不知道，查ID ID只能时数字

hashcat --help

hashcat --help|grep -i "加密模式名字"

如hashcat --help|grep -i "MD5"

-i 时 ignore case 忽略大小写；