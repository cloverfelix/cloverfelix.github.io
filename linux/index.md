# Linux


## Linux 基本使用

 - **/bin**: bin是Binary的缩写，这个目录存放着最经常使用的命令。
 - **/boot**: 这里存放的是启动Linux是使用的一些核心文件，包括一些连接文件以及镜像文件。 **(不要动)**
 - **/dev**：dev是Device（设备）的缩写，存放的是Linux的外部设备，在Linux中访问设备的方式和文件的方式是相同的。
 - ==/etc==：这个目录用来存放所有的系统管理所需要的配置文件和子目录。**重要**
 - ==/home==：用户的主目录，在Linux中，每个用户都拥有一个自己的目录，一般该目录是以用户的账号命名的。
 - `/lib`：这个目录里存放着系统最基本的动态连接库，其与Windows里的DLL文件
 - **/lost+found**：这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件。**（不要动，存放一些突然关机文件）**
 - **/media**：Linux系统会自动识别一些设备，例如U盘、光驱等等，当识别后，Linux会把识别的设备挂在到这个目录下。
 - `/mnt`：系统提供该目录是为了让用户临时挂载别的文件系统，我们看将光驱挂载在/mnt上，然后进入该目录就可以查看光驱里面的内容了。
 - ==/opt==：这是给主机额外安装软件所摆放的目录。比如你安装有一个Oracle数据库，则就可以用放在这个目录下。默认是空的。
 - **/proc**：这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统的信息。**（不用管）**
 - `/root`：该目录为系统管理员，也称作超级权限这的与用户主目录。
 - **/sbin**：s就是Super User的isi，这里存放的就是系统管理员使用的系统管理程序。
 - **/srv**：该目录存放一些服务启动之后需要提取的数据。
 - **/sys**：这是Linux2.6内核的有一个很大的变化，该目录下安装了2.6内核中新出现的一个文件系统sysfs。
 - `/tmp`：这个目录用来存放一些临时文件的。**（用完即丢的文件可以放在这个目录下）**
 - ==/usr==：这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下。类似与Windows下的progra files目录。
 - **/usr/bin**：系统用户使用的应用程序。
 - **/usr/sbin**：超级用户使用的比较高级的管理程序和系统守护程序。Super
 - **/usr/src**：内核源代码默认的放置目录。
 - **/var**：这个目录中存放着在不断扩充的东西，我们习惯将那些经常被修改的目录放在这个目录下。包括各种日志文件。
 - **/run**：是一个临时文件系统，存储系统同启动以来的信息。当系统重启时，这个目录下的文件一个被删掉或者清除。

 ## Linux常用的基本命令
 ### 目录管理
 > 绝对路径、相对路径

绝对路径的全称：F:\deskBackground\xxxx
cd：切换目录命令
./：当前目录
cd  ..：退回到上一级目录

 > ls（列出目录）

	在Linux中ls可能时最常常被使用的！
-a 参数：all，查看全部的文件，包括隐藏的文件

-l  参数：列出所有的文件，包含文件的属性和权限，没有隐藏文件
所有的Linux可以组合使用。

> cd命令 切换目录

cd 目录命令（绝对路径都是以/开头，相对路径，对于当前目录该如何寻找../../）

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210523195003.png)

>  pwd显示当前用户所在的目录

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210523195318.png)

>  mkdir 创建一个目录

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210523195523.png)

	白色代表文件。蓝色代表文件夹
	
	创建一个层级目录， 要加入一个 -p


![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210523195700.png)

> rmdir 删除目录

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210523195912.png)

如果要删除一个层级目录，则也需要加入一个参数 `-p`

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210523200023.png)

rmdir仅仅只能删除空的目录，如果下面存在文件，需要先删除文件，递归删除多个目录加入`-p`参数即可

	如果删除文件夹中的内容，可以采用 rm -f 强制删除
	
	\-r 向下递归，不管有多少级目录，一并删除。
	
	\-f 直接强行删除，没有任何提示。


> cp （复制文件或者目录）

	cp 原来的地方  新的地方

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210620205633.png)

如果是重复复制一个文件会产生覆盖现象，它会提示你是否覆盖。

> rm （移出文件或者目录）

-f 忽略不存在的文件，不会出现警告，强制删除！
-r 递归删除目录！
-i 互动，删除询问是否删除

> rm -f  / 系统中所有的文件就被删除了，删库跑路就是这么操作的！

> mv 移动文件或者目录！ 重命名文件

-f 强制移动

-u 只替换已经更新过的文件

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210523215641.png)


### 基本属性 
> 看懂文件属性

Linux系统是一种典型的多用户系统，不同的用户处于不同的地位，拥有不同的权限。为了保护系统的安全性，Linux系统对不同的用户访问同一文件（包括目录文件）的权限做了不同的规定。

在Linux中我们可以使用`ll`或者`ls –l`命令来显示一个文件的属性以及文件所属的用户和组，如：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210523222439.png)

实例中，boot文件的第一个属性用"d"表示。"d"在Linux中代表该文件是一个目录文件。

在Linux中第一个字符代表这个文件是目录、文件或链接文件等等：

-   ==当为\[ **d** \]则是目录==
    
-   ==当为\[ **\-** \]则是文件；==
    
-   ==若是\[ **l** \]则表示为链接文档 ( link file )；==如Windows系统中应用的快捷方式
    
-   若是\[ **b** \]则表示为装置文件里面的可供储存的接口设备 ( 可随机存取装置 )；
    
-   若是\[ **c** \]则表示为装置文件里面的串行端口设备，例如键盘、鼠标 ( 一次性读取装置 )。

接下来的字符中，以三个为一组，且均为『rwx』 的三个参数的组合。

	其中，\[ r \]代表可读(read)、\[ w \]代表可写(write)、\[ x \]代表可执行(execute)。
	
	要注意的是，这三个权限的位置不会改变，如果没有权限，就会出现减号\[ - \]而已。

每个文件的属性由左边第一部分的10个字符来确定（如下图）：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210523222535.png)

从左至右用0-9这些数字来表示。

	第0位确定文件类型，第1-3位确定属主（该文件的所有者）拥有该文件的权限。第4-6位确定属组（所有者的同组用户）拥有该文件的权限，第7-9位确定其他用户拥有该文件的权限。

其中：

	第1、4、7位表示读权限，如果用"r"字符表示，则有读权限，如果用"-"字符表示，则没有读权限；
	
	第2、5、8位表示写权限，如果用"w"字符表示，则有写权限，如果用"-"字符表示没有写权限；
	
	第3、6、9位表示可执行权限，如果用"x"字符表示，则有执行权限，如果用"-"字符表示，则没有执行权限。

对于文件来说，它都有一个特定的所有者，也就是对该文件具有所有权的用户。

同时，在Linux系统中，用户是按组分类的，一个用户属于一个或多个组。

文件所有者以外的用户又可以分为文件所有者的同组用户和其他用户。

因此，Linux系统按文件所有者、文件所有者同组用户和其他用户来规定了不同的文件访问权限。

在以上实例中，boot 文件是一个目录文件，属主和属组都为 root。	
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210524183724.png)




> 修改文件属性

**1、chgrp：更改文件属组**

	属主：表示属于哪一个主人
	属组：表示属于哪一个组
~~~  shell
chgrp \[-R\] 属组名 文件名
~~~
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210524184321.png)

\ -R：递归更改文件属组，就是在更改某个目录文件的`属组`时，如果加上-R的参数，那么该目录下的所有文件的`属组`都会更改。

**2、chown：更改文件属主，也可以同时更改文件属组**

~~~ shell
chown  \[–R\] 属主名 文件名  
chown  \[-R\] 属主名：属组名 文件名 
~~~
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210524184451.png)

**3、chmod：更改文件9个属性**   ==（必须掌握）==

~~~ shell
chmod  \[-R\] xyz 文件或目
~~~

Linux文件属性有两种设置方法，**一种是数字，一种是符号。**

Linux文件的基本权限就有九个，分别是**owner/group/others**三种身份各有自己的**read/write/execute**权限。

先复习一下刚刚上面提到的数据：**文件的权限字符为：『-rwxrwxrwx』**， 这九个权限是三个三个一组的！其中，我们可以使用数字来代表各个权限，各权限的分数对照表如下：

	r:4     w:2         x:1

每种身份(owner/group/others)各自的三个权限(r/w/x)分数是需要累加的，例如当权限为：\[-rwxrwx---\] 分数则是：

	- owner = rwx = 4+2+1 = 7
	
	- group = rwx = 4+2+1 = 7
	
	- others= --- = 0+0+0 = 0

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210524184817.png)


~~~ shell
chmod 770 filename
~~~
可以自己下去多进行测试！


### 文件内容查看
Linux系统中使用以下命令来查看文件内容
- **cat** 由第一行开始显示文件内容

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210525200911.png)

- **tac** 从最后一行开始显示，可以看出tac 是 cat 的倒着写

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210525200936.png)

- **nl** 显示的时候，顺道输出行号！
- **more** 一页一页的显示文件内容==按空格进行下翻页，enter代表像下一行看，:f查看行号==

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210525201208.png)

- **less** 与 more 类似，但是比more 更好的是，它可以往前翻页！==按空格进行下翻页，上下键代表翻动页面，退出使用q命令，查找字符串 /要查找的字符串 向下查询；向上查询使用？要查询的字符串 ；n 继续搜寻下一个，N 向上寻找==

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210525202243.png)

- **head** 只看头几行 ==它通过-n 参数来控制显示几行

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210525201748.png)

- **tail** 只看尾巴几行 ==也是通过-n参数来控制看那几行

可以使用**man**[命令]来查看各个命令的使用文档，如：man cp

网络配置目录：`cd /etc/sysconfig/network-scripts/`

默认网络配置文件：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210525200630.png)

> Linux链接的概念（了解即可）

Linux的链接分为两种：硬链接、软连接！

**硬链接**：A---B，假设B是A的硬链接，那么他们两个指向了同一个文件！允许一个文件拥有多个路径，用户可通过这种机制建议硬链接到一些重要文件上，防止误删。

**软链接**：类似Windows下的快捷方式，删除了原文件，快捷方式也访问不了！

创建链接 ln 命令

`touch` 命令创建文件

`echo` 输入字符串

~~~ bash
[root@iZ8vb8plggk0dqs0rr5kzfZ /]# cd home
[root@iZ8vb8plggk0dqs0rr5kzfZ home]# ls
www
[root@iZ8vb8plggk0dqs0rr5kzfZ home]# ln f1
ln: failed to access ‘f1’: No such file or directory
[root@iZ8vb8plggk0dqs0rr5kzfZ home]# touch f1            # 创建一个f1文件
[root@iZ8vb8plggk0dqs0rr5kzfZ home]# ls
f1  www
[root@iZ8vb8plggk0dqs0rr5kzfZ home]# ln f1 f2				# 创建一个硬链接 f2
[root@iZ8vb8plggk0dqs0rr5kzfZ home]# ls
f1  f2  www
[root@iZ8vb8plggk0dqs0rr5kzfZ home]# ln -s f1 f3		   # 创建一个软链接 f3
[root@iZ8vb8plggk0dqs0rr5kzfZ home]# ls
f1  f2  f3  www
[root@iZ8vb8plggk0dqs0rr5kzfZ home]# ll
total 4
-rw-r--r-- 2 root root    0 May 25 20:49 f1
-rw-r--r-- 2 root root    0 May 25 20:49 f2
lrwxrwxrwx 1 root root    2 May 25 20:49 f3 -> f1
drwx------ 3 www  www  4096 May  9 17:03 www
[root@iZ8vb8plggk0dqs0rr5kzfZ home]# echo "yeziqi hanhan" >> f1 # 给f1文件中写入一些字符串！
[root@iZ8vb8plggk0dqs0rr5kzfZ home]# cat f1				# 查看f1
yeziqi hanhan
[root@iZ8vb8plggk0dqs0rr5kzfZ home]# cat f2				# 查看f2
yeziqi hanhan
[root@iZ8vb8plggk0dqs0rr5kzfZ home]# cat f3				# 查看f3
yeziqi hanhan
[root@iZ8vb8plggk0dqs0rr5kzfZ home]# 
~~~

	删除f1之后，查看f2和f3的区别

~~~bash
[root@iZ8vb8plggk0dqs0rr5kzfZ home]# rm -rf f1
[root@iZ8vb8plggk0dqs0rr5kzfZ home]# ls
f2  f3  www
[root@iZ8vb8plggk0dqs0rr5kzfZ home]# cat f2				#f2硬链接还在
yeziqi hanhan
[root@iZ8vb8plggk0dqs0rr5kzfZ home]# cat f3				#f3（软链接、符号链接失效）快捷方式失效
cat: f3: No such file or directory
~~~


### Vim编辑器
vim通过一些插件可以实现和IDE一样的功能！

>什么是Vim编辑器

Vim是从 vi 发展出来的一个文本编辑器。代码补完、编译及错误跳转等方便编程的功能特别丰富，在程序员中被广泛使用。

简单的来说， vi 是老式的字处理器，不过功能已经很齐全了，但是还是有可以进步的地方。

vim 则可以说是程序开发者的一项很好用的工具。

所有的 Unix Like 系统都会内建 vi 文书编辑器，其他的文书编辑器则不一定会存在。

连 vim 的官方网站 (http://www.vim.org) 自己也说 vim 是一个程序开发工具而不是文字处理软件。

vim键盘图：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210525215334.png)

> 三种使用模式

基本上 `vi/vim` 共分为三种模式，分别是**命令模式（Command mode）**，**输入模式（Insert mode）**和**底线命令模式（Last line mode）**。这三种模式的作用分别是：

**命令模式：**

用户刚刚启动 vi/vim，便进入了命令模式。

此状态下敲击键盘动作会被Vim识别为命令，而非输入字符。比如我们此时按下i，并不会输入一个字符，i被当作了一个命令。

以下是常用的几个命令：

-   **i** 切换到输入模式，以输入字符。
    
-   **x** 删除当前光标所在处的字符。
    
-   **:** 切换到底线命令模式，以在最底一行输入命令。如果是编辑模式，需要先退出编辑模式！ESC！    

若想要编辑文本：启动Vim，进入了命令模式，按下i，切换到输入模式。

命令模式只有一些最基本的命令，因此仍要依靠底线命令模式输入更多命令。

**输入模式：**

在命令模式下按下i就进入了输入模式。

在输入模式中，可以使用以下按键：

-   **字符按键以及Shift组合**，输入字符
    
-   **ENTER**，回车键，换行
    
-   **BACK SPACE**，退格键，删除光标前一个字符
    
-   **DEL**，删除键，删除光标后一个字符
    
-   **方向键**，在文本中移动光标
    
-   **HOME**/**END**，移动光标到行首/行尾
    
-   **Page Up**/**Page Down**，上/下翻页
    
-   **Insert**，切换光标为输入/替换模式，光标将变成竖线/下划线
    
-   **ESC**，退出输入模式，切换到命令模式

**底线命令模式**

在命令模式下按下`:（英文冒号）`就进入了底线命令模式。

底线命令模式可以输入单个或多个字符的命令，可用的命令非常多。

在底线命令模式中，基本的命令有（已经省略了冒号）：

-   **q 退出程序**
    
-   **w 保存文件**

按ESC键可随时退出底线命令模式。
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210525223655.png)

简单的说，我们可以将这三个模式想成底下的图标来表示：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210525215513.png)

> Vim按键说明

除了上面简易示范的 i ，Esc ， :wq 之外，其实vim还有非常多的按键可以使用

**第一部分：一般模式可用的光标移动、复制粘贴、搜索替换等**

| 移动光标的方法     |                                                              |
| ------------------ | ------------------------------------------------------------ |
| h 或 向左箭头键(←) | 光标向左移动一个字符                                         |
| j 或 向下箭头键(↓) | 光标向下移动一个字符                                         |
| k 或 向上箭头键(↑) | 光标向上移动一个字符                                         |
| l 或 向右箭头键(→) | 光标向右移动一个字符                                         |
| \[Ctrl\] + \[f\]   | 屏幕『向下』移动一页，相当于 \[Page Down\]按键==(常用)==     |
| \[Ctrl\] + \[b\]   | 屏幕『向上』移动一页，相当于 \[Page Up\] 按键 ==(常用)==     |
| \[Ctrl\] + \[d\]   | 屏幕『向下』移动半页                                         |
| \[Ctrl\] + \[u\]   | 屏幕『向上』移动半页                                         |
| +                  | 光标移动到非空格符的下一行                                   |
| -                  | 光标移动到非空格符的上一行                                   |
| **数字< space>**   | 那个 n 表示『数字』，例如 20 。快捷切换光标                  |
| 0 或功能键\[Home\] | 这是数字『 0 』：移动到这一行的最前面字符处 ==(常用)==       |
| $ 或功能键\[End\]  | 移动到这一行的最后面字符处==(常用)==                         |
| H                  | 光标移动到这个屏幕的最上方那一行的第一个字符                 |
| M                  | 光标移动到这个屏幕的中央那一行的第一个字符                   |
| L                  | 光标移动到这个屏幕的最下方那一行的第一个字符                 |
| G                  | 移动到这个档案的最后一行==(常用)==                           |
| nG                 | n 为数字。移动到这个档案的第 n 行。例如 20G 则会移动到这个档案的第 20 行(可配合 :set nu) |
| gg                 | 移动到这个档案的第一行，相当于 1G 啊！==常用)==              |
| **数字< Enter>**   | 光标向下移动 n 行==(常用)==                                  |


| 搜索替换  |                                                              |
| --------- | ------------------------------------------------------------ |
| **/word** | 向光标之下寻找一个名称为 word 的字符串。例如要在档案内搜寻 vbird 这个字符串，就输入 /vbird 即可！(常用) |
| ?word     | 向光标之上寻找一个字符串名称为 word 的字符串。               |
| **n**     | 这个 n 是英文按键。代表重复前一个搜寻的动作。举例来说， 如果刚刚我们执行 /vbird 去向下搜寻 vbird 这个字符串，则按下 n 后，会向下继续搜寻下一个名称为 vbird 的字符串。如果是执行 ?vbird 的话，那么按下 n 则会向上继续搜寻名称为 vbird 的字符串！ |
| **N**     | 这个 N 是英文按键。与 n 刚好相反，为『反向』进行前一个搜寻动作。例如 /vbird 后，按下 N 则表示『向上』搜寻 vbird 。 |


| 删除、复制与粘贴 |                                                              |
| ---------------- | ------------------------------------------------------------ |
| x, X             | 在一行字当中，x 为向后删除一个字符 (相当于 \[del\] 按键)， X 为向前删除一个字符(相当于 \[backspace\] 亦即是退格键) ==(常用)== |
| nx               | n 为数字，连续向后删除 n 个字符。举例来说，我要连续删除 10 个字符， 『10x』。 |
| dd               | 删除游标所在的那一整行==(常用)==                             |
| ndd              | n 为数字。删除光标所在的向下 n 行，例如 20dd 则是删除 20 行 ==(常用)== |
| d1G              | 删除光标所在到第一行的所有数据                               |
| dG               | 删除光标所在到最后一行的所有数据                             |
| d$               | 删除游标所在处，到该行的最后一个字符                         |
| d0               | 那个是数字的 0 ，删除游标所在处，到该行的最前面一个字符      |
| yy               | 复制游标所在的那一行==(常用)==                               |
| nyy              | n 为数字。复制光标所在的向下 n 行，例如 20yy 则是复制 20 行==(常用)== |
| y1G              | 复制游标所在行到第一行的所有数据                             |
| yG               | 复制游标所在行到最后一行的所有数据                           |
| y0               | 复制光标所在的那个字符到该行行首的所有数据                   |
| y$               | 复制光标所在的那个字符到该行行尾的所有数据                   |
| p,P              | p 为将已复制的数据在光标下一行贴上，P 则为贴在游标上一行！举例来说，我目前光标在第 20 行，且已经复制了 10 行数据。则按下 p 后， 那 10 行数据会贴在原本的 20 行之后，亦即由 21 行开始贴。但如果是按下 P 呢？那么原本的第 20 行会被推到变成 30 行。==(常用)== |
| J                | 将光标所在行与下一行的数据结合成同一行                       |
| c                | 重复删除多个数据，例如向下删除 10 行，\[ 10cj \]             |
| **u**            | 复原前一个动作。==(常用)==                                   |
| \[Ctrl\]+r       | 重做上一个动作。==(常用)==                                   |

**第二部分：一般模式切换到编辑模式的可用的按钮说明**

| 进入输入或取代的编辑模式 |                                                              |
| ------------------------ | ------------------------------------------------------------ |
| **i,I**                  | 进入输入模式(Insert mode)：i 为『从目前光标所在处输入』， I 为『在目前所在行的第一个非空格符处开始输入』。==(常用)== |
| a,A                      | 进入输入模式(Insert mode)：a 为『从目前光标所在的下一个字符处开始输入』， A 为『从光标所在行的最后一个字符处开始输入』。==(常用)== |
| o,O                      | 进入输入模式(Insert mode)：这是英文字母 o 的大小写。o 为『在目前光标所在的下一行处输入新的一行』；O 为在目前光标所在处的上一行输入新的一行！==(常用)== |
| **r,R**                  | 进入取代模式(Replace mode)：r 只会取代光标所在的那一个字符一次；R会一直取代光标所在的文字，直到按下 ESC 为止；==(常用)== |
| **\[Esc\]**              | 退出编辑模式，回到一般模式中==(常用)==                       |

**第三部分：一般模式切换到指令行模式的可用的按钮说明**

| 指令行的储存、离开等指令                                     |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| :w                                                           | 将编辑的数据写入硬盘档案中==(常用)==                         |
| :w!                                                          | 若文件属性为『只读』时，强制写入该档案。不过，到底能不能写入， 还是跟你对该档案的档案权限有关啊！ |
| :q                                                           | 离开 vi ==(常用)==                                           |
| :q!                                                          | 若曾修改过档案，又不想储存，使用 ! 为强制离开不储存档案。    |
| 注意一下啊，那个惊叹号 (!) 在 vi 当中，常常具有『强制』的意思～ |                                                              |
| **:wq**                                                      | 储存后离开，若为 :wq! 则为强制储存后离开 ==(常用)==          |
| ZZ                                                           | 这是大写的 Z 喔！若档案没有更动，则不储存离开，若档案已经被更动过，则储存后离开 |
| :w \[filename\]                                              | 将编辑的数据储存成另一个档案（类似另存新档）                 |
| :r \[filename\]                                              | 在编辑的数据中，读入另一个档案的数据。亦即将 『filename』 这个档案内容加到游标所在行后面 |
| :n1,n2 w \[filename\]                                        | 将 n1 到 n2 的内容储存成 filename 这个档案。                 |
| :! command                                                   | 暂时离开 vi 到指令行模式下执行 command 的显示结果！例如 『:! ls /home』即可在 vi 当中看 /home 底下以 ls 输出的档案信息！ |
| **:set nu**                                                  | 显示行号，设定之后，会在每一行的前缀显示该行的行号           |
| :set nonu                                                    | 与 set nu 相反，为取消行号！                                 |

以上标绿的是要熟练掌握的，其余的做了解即可！


### 账号管理

实现用户账号的管理，要完成的工作主要有如下几个方面：

-   用户账号的添加、删除与修改。
    
-   用户口令的管理。
    
-   用户组的管理。

> useradd 命令 添加用户

useradd -选项 用户名

-m：自动创建这个用户的主目录--> /home/用户名

	参数说明：
	
	   选项 :
	   \-c comment 指定一段注释性描述。
	
	   \-d 目录 指定用户主目录，如果此目录不存在，则同时使用-m选项，可以创建主目录。
	
	   \-g 用户组 指定用户所属的用户组。
	
	   \-G 用户组，用户组 指定用户所属的附加组。
	
	   \-m　使用者目录如不存在则自动建立。
	
	  \-s Shell文件 指定用户的登录Shell。
	
	  \-u 用户号 指定用户的用户号，如果同时有-o选项，则可以重复使用其他用户的标识号。


	  用户名 :
	  
	 	指定新账号的登录名。

~~~bash
[root@iZ8vb8plggk0dqs0rr5kzfZ home]# useradd -m wei
[root@iZ8vb8plggk0dqs0rr5kzfZ home]# ls
wei  www
~~~

理解一下本质：Linux中一切皆文件，这里的添加用户说白了就是往某一个文件中写入用户信息了！/etc/passwd

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210526200217.png)

> 删除用户 userdel

userdel -r wei 删除用户的时候将他的目录页一并删掉！

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210526200716.png)

> 修改用户 usermod

修改用户 usermod 对应修改的内容  修改的用户

~~~xhsell
usermod -d /home/test wei
~~~

	在进行修改某一个用户时，也要修改其文件的时候，应当先创建要移动到的文件，否则可能当前目录不显示移动后的文件，但是在配置文件中是存在的！


​	
>Linux 下如何切换用户

root用户

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210526203849.png)

- 1.切换用户的命令为：su username 【username是你的用户名哦】

- 2.从普通用户切换到root用户，还可以使用命令：sudo su

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210526204016.png)

- 3.在终端输入exit或logout或使用快捷方式ctrl+d，可以退回到原来用户，其实ctrl+d也是执行的exit命令

- 4.在切换用户时，如果想在切换用户之后使用新用户的工作环境，可以在su和username之间加-，例如：【su - root】

**$表示普通用户**

#表示超级用户，也就是root用户


>与用户的密码设置问题

我们一般它通过root创建用户的时候！要配置密码

Linux上输入密码时不会显示的，正常输入即可！

在公司中，一般拿不到公司服务器的root权限，都是有一些分配的账号！

> 锁定账户

root一旦冻结这个账号，这个人就登录不上系统了！

~~~bash
passwd -l wei			 #锁定之后这个用户也就不能登录了
passwd -d wei			#没有密码也不能登录
~~~


### 用户组管理

属主（属于那一个主人）、属组

每个用户都有一个用户组，系统可以对一个用户组中的所有用户进行集中管理（开发、测试、运维、root）。不同Linux 系统对用户
组的规定有所不同，如Linux下的用户属于与它同名的用户组，这个用户组在创建用户时同时创建。

用户组的管理涉及用户组的添加、删除和修改。==组的增加、删除和修改实际上就是对/etc/group文件的更新。

>创建一个用户组

~~~bash
groupadd wei
~~~

创建完用户组后可以得到一个Id，这个Id是可以指定的！`-g 520` 如果不指定就是自增1

 > 用户如果要切换用户组怎么办呢？

~~~
# 登录当前用户  wei
$ newgrp root
~~~

 这样转换的前提是你得需要有这个权限，否则切换不成功！

 > 删除用户组 groupdel

~~~bash
groupdel wei
~~~


 > 修改与用户组的权限信息和名字 groupmod  

	-g：修改端口号
	-n：修改用户名字


 > 拓展；文件的查看！（了解即可）

`/etc/passwd`

~~~java
用户名:口令:用户标识号:组标识号:注释性描述:主目录:登录Shell
~~~

这个文件中的灭有一行都代表这一个用户，我们可以用从这里看出这个用户的主目录在哪里，可以看到属于哪一个组！

登录口令：把真正的加密后的用户口令字存放到/etc/shadow文件中

用户组的所有信息都存放在/etc/group文件中


### 磁盘管理

> df （列出文件系统整体的磁盘使用量） du（检查磁盘空间的使用量）

df

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210526211917.png)

du

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210526212348.png)


> Mac或者想要使用Linux挂载我们的ixie本地磁盘或者文件！


挂载：使用`mount`命令


![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210526212807.png)

卸载：`unmount -f ` [挂载位置] 强制卸载

### 进程管理
Linux中一切皆文件

==(文件: 读、写、执行（查看，创建，删除，移动，复制，编辑），权限（用户，用户组），系统（磁盘，进程））==
> 什么是进程（基本概念）

1. 在Linux中，每一个进程都是有自己的一个进程，每一个进程都有一个id号
2. 每一个进程，都有一个父进程
3. 进程可以拥有两种方式存在：前台！后台运行
4. 一般的话服务都是后台运行的，基本的程序都是前台运行的！


> 命令

**ps** 查看当前系统中正在执行的各种进程信息

ps -xxx:
- a 显示当前终端运行的所有进程信息(当前的进程是一个)
- u 以用户的信息显示进程
- x 显示后台运行进程的参数!

~~~bash
ps -aux 查看所有的进程
ps -aux |grep mysql
# |  在Linux中这个叫做管道符
# grep 查找文件中符合条件的字符串(相当于过滤)
~~~

对于我们来说, 这里目前只需要记住一个命令即可  ps -xx|grep 进程名字!过滤进程信息!

**ps -ef: 可以查看到父进程的信息**
~~~bash
ps -ef|grep mysql   #  看父进程我们一般可以通过目录树结构来查看！

#  进程树
pstree -pu
		-p    显示父id
		-u    显示用户组
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210529162819.png)

结束进程： 杀掉进程，等价与Windows结束任务

kill -9 进程的id

~~~bash
kill -9 进程的id
~~~

表示强制结束进程

### 环境安装
安装软件有一般有三种方式：
- rpm（Jdk：在线发布一个SpringBoot项目）
- 解压缩（tomcat，启动并通过外网访问，也就是发布网站）
- yum在线安装（docker：直接运行跑起来docker就可以！）

#### JDK安装
1. 下载JDK rpm。去Oracle官网下载即可！
2. 安装Java环境

~~~文字
# 检测当前系统是否存在Java环境！  Java -version
# 如果有的话就需要先卸载
# rpm -qa|grep jdk   # 检测JDK版本信息
# rpm -e --nodeps jdk所在文件夹    # --nodeps  代表强制删除

# 卸载完毕后即可安装JDK
# rpm -ivh rpm包

# 配置环境变量
~~~

配置环境变量：`etc/profile`在文件的最后面增加Java的配置和Windows安装环境变量一样
~~~java
JAVA_HOME=/usr/java/jdk															  #  Java根路径
CLASSPATH=%JAVA_HOME%/lib:%JAVA_HOME%/jre/lib		   # 类路径文件
PATH=$JAVA_HOME/bin:$JAVA_HOME/jre/bin               			# 环境变量文件
export PATH CLASSPATH JAVA_HOME
~~~

让这个配置文件生效！`source /etc/profile`


	确保Linux的防火墙端口是开启的，如果是阿里云，需要保证阿里云的安全组策略是开放的！

~~~shell
# 查看firewall服务状态  
systemctl status firewalld  
  
# 开启、重启、关闭、firewalld.service服务  
# 开启  
service firewalld start  
# 重启  
service firewalld restart  
# 关闭  
service firewalld stop  
  
# 查看防火墙规则  
firewall-cmd --list-all    # 查看全部信息  
firewall-cmd --list-ports  # 只看端口信息  
  
# 开启端口  
开端口命令：firewall-cmd --zone=public --add-port=80/tcp --permanent  
重启防火墙：systemctl restart firewalld.service  
  
命令含义：  
--zone #作用域  
--add-port=80/tcp  #添加端口，格式为：端口/通讯协议  
--permanent   #永久生效，没有此参数重启后失效
~~~

#### Tomcat安装

1. 安装好了Java环境后我们可以测试下Tomcat！准备好Tomcat的安装包！
2. 将文件移动到/usr/tomcat/下，并解压！
~~~shell
root@kuangshen kuangshen]# mv apache-tomcat-9.0.22.tar.gz /usr  
[root@kuangshen kuangshen]# cd /usr  
[root@kuangshen usr]# ls  
apache-tomcat-9.0.22.tar.gz  
[root@kuangshen usr]# tar -zxvf apache-tomcat-9.0.22.tar.gz   # 解压
~~~
3. 运行Tomcat，进入bin目录，和我们以前在Windows下看的都是一样的
~~~shell
# 执行：startup.sh -->启动tomcat  
# 执行：shutdown.sh -->关闭tomcat  
./startup.sh  
./shutdown.sh
~~~
4. 确保Linux的防火墙端口是开启的，如果是阿里云，需要保证阿里云的安全组策略是开放的！
~~~shell
# 查看firewall服务状态  
systemctl status firewalld  
  
# 开启、重启、关闭、firewalld.service服务  
# 开启  
service firewalld start  
# 重启  
service firewalld restart  
# 关闭  
service firewalld stop  
  
# 查看防火墙规则  
firewall-cmd --list-all    # 查看全部信息  
firewall-cmd --list-ports  # 只看端口信息  
  
# 开启端口  
开端口命令：firewall-cmd --zone=public --add-port=80/tcp --permanent  
重启防火墙：systemctl restart firewalld.service  
  
命令含义：  
--zone #作用域  
--add-port=80/tcp  #添加端口，格式为：端口/通讯协议  
--permanent   #永久生效，没有此参数重启后失效
~~~

域名解析完毕后，如果端口是80 -http或者443 -https，可以直接访问，如果是9000 8080，就需要通过Apache或者Nginx做一下反向代理即可，配置文件即可。


