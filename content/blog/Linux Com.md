+++
title = 'Linux Commands'
+++
1. `mkdir` 新建文件夹 make directory
2. `ls` 列出当前目录内容 list
3. `pwd` = print working directory 展示当前工作文件夹
4. `cd` = change directory 
5. `cp` = copy
6. `mv` = move
7. `rm` = remove
8. `cat` = concatenate (连接)  在终端显示文本内容
9. `tail -n 2 file` 显示文件末尾
10. `touch` 新建文件
11. `vim`生成Makefile 文件
12. 保存`Makefile`步骤如下：

    1. 按 `i` 键进入插入模式 (Insert Mode)**，此时可以编辑文本。
    2. 输入完内容后，**按 `Esc` 键**退出插入模式，回到**正常模式 (Normal Mode)**。
    3. 输入冒号 `:`，进入**命令行模式 (Command-Line Mode)**，光标会跳到窗口底部。
    4. 输入 `wq`，然后**按 `Enter` 键**。
       - `w` 代表 **write** (保存)
       - `q` 代表 **quit** (退出)
       - 组合 `:wq` 就是“保存并退出”。

    **口诀：`i` (编辑) -> `Esc` (退出编辑) -> `:wq` (保存退出) -> `Enter` (执行)。**

    **(可选) 如果不想保存，直接退出**：按 `Esc` 后，输入 `:q!` 然后按 `Enter`（强制退出不保存）。

13. 

    ```bash
    gcc -E hello.c -o hello.i # 预处理
    gcc -S hello.i -o hello.s # 编译
    gcc -c hello.s -o hello.o # 汇编
    gcc hello.o -o hello # 链接
    ./a.out #运行
    ```
    
    **预处理**过程做了什么：
    
    1. 展开所有的宏定义（`#define`）。
    2. 处理所有的条件预编译指令（如 `#if`, `#ifdef`）。
    3. 递归地包含（`#include`）所指明的头文件（如 `stdio.h`），将其内容直接插入到 `.i` 文件中。
    4. 删除所有的注释 `//` 和 `/* ... */`。
    5. 结果：生成一个经过预处理后的源代码文件** (`hello.i`)。你可以用文本编辑器打开它看看，会发现文件变得非常大，因为 `stdio.h` 等头文件的内容都被包含了进来，但你的 `main` 函数代码还在最后。

# 内核模块

```bash
make # 编译
sudo insmod helloworld.ko # 装载模块
dmesg  # 查看控制台输出
lsmod # 查看模块
lsmod | grep 文件名 #查看指定模块
sudo rmmod helloworld  # 卸载模块
```



# 云计算实验课

1. `sudo gedit /etc/hostname ` 修改机器名

2. 查看防火墙状态：`sudo ufw status  `

   打开防火墙：`sudo ufw enable  `

   关闭防火墙：`sudo ufw disable`

3. ```bash
   cd /usr
   rz # 上传文件
   nohup java -jar smxz-0.0.1-SNAPSHOT.jar > Log.log 2>&1 & # 挂在后台
   ```
4. 查看后台运行的任务 **`jobs`**
5. 查看java项目的PID号 **`jps`**
6. 使用kill杀死进程，中断java程序 **`kill -9 PID号`**