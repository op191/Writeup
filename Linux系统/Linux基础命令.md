打开LInux终端默认以home目录为当前目录

处理文件和目录的命令用-r（如rm,cp,grep)

处理权限的命令（如chmod,chown)用-R

命令：command [-options]  parameter

command:命令本体

options:可选选项，控制命令行为细节

parameter:可选参数，控制命令的指向对象，如路径


当前工作目录，若是往上跳，不用command时，只能唯一跳到/，往下跳一级可以直接输入目录名字，即parameter（只能往下跳一级哦，若是跳两级以上则需要cd命令）

Linux对字母大小写很敏感；window对字母大小写不敏感；
在Linux ,将Desktop写成desktop系统会识别成两个东西，即两个目录


ssh命令（远程登陆）：
ssh 用户名@ip 如  ssh root@192.168.132.1
指定端口：
ssh -p 端口号 用户名@ip  如：ssh -p 77 iojn@192.168.135.1

scp命令（基于ssh的远程文件传输）（secure copy protocol安全复制协议）

传整个  文件夹  必须加-r
指定端口加  -P  (注意与上面远程登陆小 p 区分开)
scp [选项] 源文件 目标位置
如：本地-->远程（上传）
scp 本地文件 用户名@服务器ip:/目标路径
远程-->本地
scp 用户名@服务器ip:/远程文件 本地路径
如 scp root@192.168.153.2:/root/log.txt ./


ls命令：
-a 展示隐藏内容（all）
-l 以列表形式展示（list）
-h 需要搭配-l以显示文件大小（human-readable）
需要同时使用时-a -l -h=-alh(不分顺序)

pwd命令（Print Work Directory)：
打印当前所在工作目录，无option无parameter，直接pwd;

cd:
..表示上一级目录；
.表示当前目录；
../..  回退两级目录；

mkdir(make directory)创建一个新目录即文件夹
mkdir -p 路径
-p可选，一次性创建多个层级的目录需写上
如：mkdir -p pro/oo/66.txt


cat命令（concatenate连接）
-n 显示行号  -b只给非空行编号 -A显示隐藏符
可以查看文件全部内容，（一次性将全部文件内容显示）；
可以拼接文件成一个文件；
如cat pp.txt uu.txt > all.txt:  将pp.txt和uu.txt文件拼接成all.txt,如果没有all.txt会自动创建

less命令：
分页查看文件内容，按q退出，按 空格 下一页，按 b 上一页，

touch命令：
创建一个新文件；
touch 路径


cp(copy)命令：
cp [-r] 参数1 参数2
-r (recursive递归)，用于复制文件夹时使用
将文件从参数1复制到参数2，即从把前面复制到后面


mv (move)命令，类似剪切：
mv 参数1 参数2
参数1为被移动的文件，参数2为移动到的位置，都为路径


rm命令（remove):
rm [-r -f ]参数1 参数2 ....参数n
当删除多个文件或者只删除文件夹时需要-r

通配符  *
test*:表示任意以text为开头的文件或文件夹
*test:表示任意以test为结尾的文件或文件夹
*test*:表示任意包含test的文件夹或文件
不是只文件内容有test,而是文件名带test的文件或文件夹；

grep命令（是最核心的文本搜索工具）：


对于有x权限的文件夹即execute(执行)就可以用 cd命令进入该文件夹


chmod命令（change mode改变模式）：
该命令用于修改文件或文件夹的权限信息，r,w,x,为权限信息
chmod [-R] 权限 文件或文件夹
-R对文件夹内全部内容应用同样的操,一定是-大R,大R表示recursive递归，小r表示read
语法：chmod [-R] 数字 路径
rwx顺序定死，r=4,  w=2 ,  x=1
如 chmod 751  表示rwx r-x --x 第一个数字代表用户所有权限和，第二个数字代表用户组，第三个数字代表其他用户

chown命令：
修改文件或文件夹的所属用户，只能root权限执行

chown [-R] user:group 路径；
如 chown root pp.txt 修改文件为root所属
     chown :op pp.txt 修改文件为op组所属
     chown  admin:pp 修改文件为用户admin所属和pp组所属；
用户和用户组可以同时出现，也可以单独出现；


vim命令（文件编辑器）：
vim  file.txt
i 进入编辑模式
Esc 退出编辑
wq 保存退出
q!  不保存强制退出

akw命令（按列处理文本）常用场景：取某一段数据；
如awk'{print $1, 3}' file.txt  打印第1列和第3列


