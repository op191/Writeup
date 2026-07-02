PowerShell方式（对象管道）
powershell

# PowerShell: 对象→对象
Get-Process | Where-Object { $_.Name -eq "ssh" }

执行过程：

    Get-Process 生成一组进程对象（每个进程有 Name、ID、CPU 等属性）

    | 把整个对象列表传给 Where-Object

    Where-Object 对每个对象进行判断：$_.Name -eq "ssh"（筛选名称等于ssh的）

    输出符合条件的对象（仍然保留所有属性）

优势：你不用解析文本，直接操作属性，精确且不易出错。
三、PowerShell管道核心概念：$_（当前对象）

$_ 是PowerShell管道的灵魂，代表“管道中当前正在处理的那个对象”。
理解 $_
powershell

Get-Process | Where-Object { $_.CPU -gt 100 }

    Get-Process 产生很多进程对象

    管道一个一个把这些对象传给 Where-Object

    Where-Object 每次取一个对象，用 $_ 代表它

    $_.CPU -gt 100 检查这个进程的CPU是否大于100

    保留符合条件的，扔掉不符合的

类比Python：
python

for process in Get_Process():   # 伪代码
    if process.CPU > 100:
        print(process)

$_ 就是循环里的 process。
四、管道最常用的4个场景
场景1：筛选（Where-Object）
powershell

Get-Service | Where-Object { $_.Status -eq "Running" }

只显示当前正在运行的服务。
场景2：取特定字段（Select-Object）
powershell

Get-Process | Select-Object Name, CPU, WorkingSet

只显示进程的名称、CPU、内存占用（类似 ps -eo pid,comm）。
场景3：排序（Sort-Object）
powershell

Get-Process | Sort-Object -Property CPU -Descending | Select-Object -First 5

按CPU倒序排序，取前5个（相当于 top）。
场景4：转发给循环（ForEach-Object）
powershell

Get-LocalUser | ForEach-Object { Write-Host "用户: $($_.Name)" }

对每个用户输出一句“用户: xxx”。
