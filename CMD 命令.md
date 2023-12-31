# CMD 命令

[bat 脚本](https://www.wolai.com/kEmTrMVTZikk9ZBADStnX "bat 脚本")

# 1. 基本操作

列出所有命令：

```bash
help
```

查看命令帮助：

```bash
<命令> /?

```

```bash
help <命令>
```

清屏：

```bash
cls
```

## 1.1 外部调用 cmd

运行后关闭窗口：

```bash
cmd /c <cmd命令>
```

运行后保留窗口：

```bash
cmd /k <cmd命令>
```

# 2. 文件浏览

## 2.1 dir

查看当前目录下的文件：

```bash
dir
```

查看指定目录下的文件：

```bash
dir <路径>
```

> 注意：路径有空格需要加双引号。

参数：

| ******/b**   | 只显示文件名，不显示属性 |
| ------------ | ------------ |
| ******/s**   | 显示子文件夹       |
| ******/w**   | 分列显示，不显示属性   |
| ******/ad**  | 只显示文件夹       |
| ******/ah**  | 显示隐藏文件       |
| ******/as**  | 显示系统文件       |
| ******/a-s** | 显示非系统s文件     |

通配符：

-   `*` 任意字符
-   `?` 任意一个字符
-   匹配所有 `.txt` 后缀的文件：
    ```bash
    dir *.txt
    ```

## 2.2 cd

**相对路径：**

-   定位下级目录：
    ```bash
    cd <目录>
    ```
-   定位到上级目录：
    ```bash
    cd ..
    ```
-   定位到上级目录下的 xx 目录：
    ```bash
    cd ..\xx
    ```
-   定位到盘符根目录：
    ```bash
    cd \
    ```
-   定位到根目录下的 xx 文件夹：
    ```bash
    cd \xx
    ```

**绝对路径：**

-   定位到指定路径：
    > 只能是当前盘
    ```batch
    cd D:\test
    ```
    切换当前盘符：
    ```batch
    :: 切换到 D 盘
    D:
    ```
-   定位到其他盘的路径：
    ```bash
    cd /d <路径>
    ```

### 2.2.1 **保存定位路径**

保存路径：

```batch
pushd <路径>
```

还原路径：

```batch
popd
```

## 2.3 tree

查看目录结构（树状）：

> 包含多级目录。

```batch
tree
```

查看指定路径下的目录结构：

```batch
tree <路径>
```

查看结构包括文件：

```batch
tree /f
```

## 2.4 type

查看文件内容（文本）：

```batch
type <文件路径>
```

# 3. 查找

## 3.1 find

> 作用：查找符合条件的行。

在文件中查找内容

```bash
find "查找文本" <文件路径>
```

对命令的结构进行查找：

```bash
<命令> | find "查找文本"
```

查找文件：

```bash
dir /s /b | find "查找文本"
```

参数：

| ******/i** | 忽略大小写 |
| ---------- | ----- |
| ******/v** | 反向查找  |
| ******/n** | 显示行号  |

## 3.2 findstr

说明：

-   用法基本同 find，区别是 findstr 可以使用部分正则表达式。
-   注意：findstr 中的空格表示或运算。



在文件中查找内容：

```bash
findstr 查找内容 <文件路径>
```

对命令的结果进行过滤：

```bash
<命令> | findstr 查找内容
```

或运算：

```bash
findstr "关键词1 关键词2" <文件路径>
```

参数：

| ******/b** | 行首匹配  |
| ---------- | ----- |
| ******/e** | 行尾匹配  |
| **/v**     | 反向匹配  |
| ******/i** | 忽略大小写 |
| ******/n** | 显示行号  |

正则表达式：

| **正则**             | **作用**   |
| ------------------ | -------- |
| ******.**          | 查找任意单个字符 |
| ******.****\***    | 查找任意字符   |
| ******^**          | 行的开始     |
| ******\$**         | 行的结束     |
| **\\<**            | 字符的开始    |
| **\\>**            | 字符的结尾    |
| ******^\[0-9]+\$** | 匹配纯数字的行  |
| ******\[^0-9]**    | 过滤纯数字的行  |

# 4. 文件操作

## 4.1 创建

创建文件夹：

> 文件名有空格和 `&` 需要用引号包裹。

```bash
md 文件夹1 文件夹2
```

创建空文件：

```bash
cd.>文件名.txt

```

```bash
type nul>文件名.txt
```

```bash
echo on>文件名.txt
```

输入多行文本并保存：

```bash
copy con 文件名.txt
```

```bash
type con>文件名.txt
```

```bash
more>文件名.txt
```

合并文件内容：

> 将一个文件1的内容追加到文件2后面。

```bash
type 文件1.txt>>文件2.txt
```

### 4.1.1 符号链接

创建符号链接（默认为文件）：

```bash
mklink 新地址 原地址
```

| **参数** | **作用**    |
| ------ | --------- |
| **/j** | 为目录创建符号链接 |

C盘软件搬家：

1.  将C盘目录移动至D盘（手动操作）。
2.  创建符号链接（管理员权限执行）。

```bash
mklink /j "C:\Program Files\Adobe\Adobe Premiere Pro 2018" "D:\Program Files\Adobe\Adobe Premiere Pro 2018"
```

## 4.2 删除

删除空目录

```bash
rd 目录名
```

| **参数** | **作用**       |
| ------ | ------------ |
| **/s** | 支持多级目录       |
| **/q** | 安静模式，不确认是否删除 |

删除文件：

```bash
del 文件路径
```

| **参数** | **作用**  |
| ------ | ------- |
| **/s** | 包括所有子文件 |
| **/q** | 不确认是否删除 |
| **/p** | 删除文件前确认 |

## 4.3 复制

### 4.3.1 copy

复制文件：

```bash
copy 文件路径 目标路径
```

| **参数** | **作用**   |
| ------ | -------- |
| **/y** | 直接覆盖，不提示 |
| **/b** | 表示为二进制文件 |

合并文件内容：

```bash
copy /b 1.txt+2.txt 3.txt
```

合并图片和压缩包：

```bash
copy /b 1.jpg+2.zip 3.jpg
```

### 4.3.2 xcopy

| **参数**   | **作用**                                |
| -------- | ------------------------------------- |
| /s       | 复制目录和子目录，不包括空目录                       |
| /e       | 复制目录和子目录，包括空目录                        |
| /t       | 创建目录结构，不复制文件                          |
| /u       | 复制已经存在于目标中的文件                         |
| /f       | 显示源文件路径和目标文件路径                        |
| /q       | 复制时不显示文件名                             |
| /d:m-d-y | 复制在指定日期及以后更改的文件，&#xA;默认复制源文件比目标文件新的文件 |

## 4.3 修改

重命名：

> 可以使用通配符：如 `*`。&#x20;

```bash
ren 原文件 新名称
```

移动文件（可用于重命名）：

```bash
mode 原路径 新路径
```

### 4.3.1 attrib

设置文件属性：

```bash
attrib [+h] / [-h] <path>
```

| **标识**    | **含义** |
| --------- | ------ |
| ******+** | 添加属性   |
| **-**     | 清除属性   |
| **r**     | 只读文件属性 |
| **h**     | 隐藏文件属性 |



# 5. 系统相关

卸载产品密钥：

```bash
slmgr /upk
```

## 5.1 查看信息

查看系统版本：

```bash
winver
```

查看当前日期：

```bash
date /t
```

查看当前时间：

```bash
time /t
```

查看环境变量的路径：

```bash
where 环境变量名
```

## 5.2 关机、注销

用法：

```bash
shutdown -s -t 0
```

| **参数**        | **作用**         |
| ------------- | -------------- |
| **/a**        | 取消关机命令         |
| **/p**        | 立即关闭计算机        |
| **/s**        | 关闭计算机（默认 60 秒） |
| **/r**        | 重启             |
| ******/t**    | 指定延时时间（秒）      |
| ******/f**    | 强制执行，不等待程序关闭   |
| /**h**        | 休眠             |
| /**l**        | 锁定             |
| ******-c 文本** | 添加注释           |

注销：

```bash
logoff
```

## 5.3 进程管理

### 5.3.1 taskkill

结束进程：

> 进程名可以使用通配符。

```bash
taskkill /im 进程名
```

| **参数**      | **作用**    |
| ----------- | --------- |
| /t          | 结束进程和子进程  |
| /f          | 强制结束      |
| /im <进程名>   | 根据进程名查找进程 |
| /pid \<pid> | 根据pid查找进程 |

### 5.3.2 tasklis

查看运行中的进程：

```bash
tasklist
```

| **参数**  | **作用**           |
| ------- | ---------------- |
| **/v**  | 显示详细信息           |
| **/fo** | 指定输出格式（list,csv） |
| **/fi** | 筛选器              |

筛选器：

| **筛选器名称**   | **有效运算符**              | **有效值**                                            |
| ----------- | ---------------------- | -------------------------------------------------- |
| STATUS      | eq, ne                 | RUNNING \| SUSPENDED&#xA;NOT RESPONDING \| UNKNOWN |
| IMAGENAME   | eq, ne                 | 进程名                                                |
| PID         | eq, ne, gt, lt, ge, le | PID 值                                              |
| SESSION     | eq, ne, gt, lt, ge, le | 会话编号                                               |
| SESSIONNAME | eq, ne                 | 会话名称                                               |
| MEMUSAGE    | eq, ne, gt, lt, ge, le | 内存使用(以 KB 为单位)                                     |
| USERNAME    | eq, ne                 | 用户名，格式为\[域\\]用户                                    |
| SERVICES    | eq, ne                 | 服务名称                                               |
| WINDOWTITLE | eq, ne                 | 窗口标题                                               |
| 模块          | eq, ne                 | DLL 名称                                             |

## 5.4 用户管理

查看用户列表：

```bash
net user
```

新建用户：

```bash
net user 用户名 密码 /add
```

删除用户：

```bash
net user 用户名 密码
```

修改密码：

```bash
net user 用户名 密码
```

查看当前用户：

```bash
whoami
```

## 5.5 修改文件权限

```perl
cacls <文件路径> /g 用户名:权限
```

```perl
echo y | cacls <文件路径> /g 用户名:权限 > nul
```

权限：

| **权限**    | **含义** |
| --------- | ------ |
| ******R** | 读取     |
| ******W** | 写入     |
| ******C** | 更改     |
| ******F** | 完全控制   |

参数：

| **参数** | **作用** |
| ------ | ------ |
| **/t** | 包括子文件  |
| **/p** | 替换     |
| **/d** | 拒绝权限   |

## 5.6 注册表操作

### 5.6.1 添加项

添加目录：

```bash
reg add "注册表路径"
```

添加字符串值：

```bash
reg add "注册表路径" /v 字符串 /d "值"
```

参数：

| **/f**  | 不提示是否覆盖 |
| ------- | ------- |

转义：

-   `\"`：转义引号。
-   `%%`：转义`%`。

### 5.6.2 删除项

删除目录：

```perl
reg delete "注册表路径"
```

删除字符串项：

```perl
red delete "注册表路径" /v 字符串
```

参数：

| **/f** | 不提示，直接删除 |
| ------ | -------- |

# 6. 终端相关

算术运算：

```bash
set /a <表达式>
```

快捷键：

| **快捷键**       | **作用**               |
| ------------- | -------------------- |
| **Ctrl+C**    | 终止正在运行的命令            |
| **Ctrl+Home** | 从当前位置开始，清除前面的字符      |
| **Ctrl+End**  | 从当前位置开始，清除后面的字符      |
| **esc**       | 清空当前命令               |
| **tab**       | 自动补全文件名              |
| **F3**        | 恢复上一条命令，在被删除部分后，可以复原 |
| **F7**        | 选择历史输入的命令            |
| **上方向键**      | 切换到上一条命令             |
| **右方向键**      | 插入上一条命令，单个字符的出现      |
| **鼠标右键**      | 复制、粘贴                |

## 6.1 color

改变文字颜色：

```bash
color c
```

改变文字和背景颜色：

> 前为背景色，后为文字色。

```bash
color fc
```

恢复默认颜色：

```bash
color
```

颜色代码：

| **0** | 黑色  | **8** | 灰色   |
| ----- | --- | ----- | ---- |
| **1** | 蓝色  | **9** | 淡蓝色  |
| **2** | 绿色  | **A** | 淡绿色  |
| **3** | 浅绿色 | **B** | 淡浅绿色 |
| **4** | 红色  | **C** | 淡红色  |
| **5** | 紫色  | **D** | 淡紫色  |
| **6** | 黄色  | **E** | 淡黄色  |
| **7** | 白色  | **F** | 亮白色  |

## 6.2 prompt

更改命令行提示信息：

```powershell
prompt <提示文本>
```

特殊字符：

| **\$A**  | & (与号)              |
| -------- | ------------------- |
| **\$B**  | \&#124; (坚线)        |
| **\$C**  | ( (左括号)             |
| **\$D**  | 当前日期                |
| **\$E**  | 转义码(ASCII码 27)      |
| **\$F**  | ) (右括号)             |
| **\$G**  | > (大于号)             |
| **\$H**  | Backspace (删除前一个字符) |
| **\$L**  | < (小于号)             |
| **\$N**  | 当前驱动器               |
| **\$P**  | 当前驱动器及路径            |
| **\$Q**  | = (等号)              |
| **\$S**  | (空格)                |
| **\$T**  | 当前时间                |
| **\$V**  | Windows 版本号         |
| **\$\_** | 回车换行符               |
| **\$\$** | \$ (美元符号)           |

# 7. 网络相关

[ipc 漏洞](https://www.wolai.com/kJmUEQ5aKQp81sQWoBeEUx "ipc 漏洞")

打开网络连接设置：

```bash
ncpa.cpl
```

查看网卡接口ip地址：

```bash
arp -a
```

查看网卡信息：

```bash
ipconfig
```

## 7.1 ping

> 测试网络连通性。

| **参数**     | **作用**   |
| ---------- | -------- |
| ******/t** | 无限次执行    |
| ******/n** | 执行次数     |
| ******/w** | 超时时间（毫秒） |

## 7.2 管理共享

查看共享资源：

```bash
net share
```

取消共享：

```bash
net share 共享名 /d
```

查看连接的共享：

```bash
net use
```

中断连接的共享：

```bash
net use * /del /y
```

使用telnet检测端口是否开放：

```bash
telnet ip地址 端口号
```

## 7.3 netstat

查看占用端口号的进程：

```bash
netstat -ano | find "端口号"
```

| **参数**     | **作用**         |
| ---------- | -------------- |
| ******-a** | 显示所有连接和监听      |
| ******-n** | 以数字的形式显示地址和端口号 |
| ******-o** | 显示进程pid        |

## 7.4 禁用网络连接

禁用网络：

```bash
netsh interface set interface (网络名) disabled
```

恢复网络：

```pl-sql
netsh interface set interface (网络名) enabled
```

## 7.5 wifi相关

查看连接过的wifi：

```pl-sql
netsh wlan show profile
```

查看wifi密码：

```bash
netsh wlan show profile name=(wifi名称) key=clear
```

断开wifi连接：

```bash
netsh wlan disconnect
```

连接wifi：

```bash
netsh wlan connect name=(wifi名)
```

