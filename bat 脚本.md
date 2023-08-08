# bat 脚本

# 1. 基本结构

基本结构：

```batch
@echo off 

...

pause
```

相关命令：

-   `@echo off`：关闭回显
-   `@命令`：不显示执行的命令，只显示结果
-   `pause`：暂停

暂停延时：

-   \`\`

延时命令（单位：秒）：

```batch
timeout /t 3
```

**窗口相关：**

更改窗口标题：

```git
title 窗口标题
```

修改窗口大小（最小值：20,1）：

```batch
mode 90,30
```

关于 `^` 的两个作用：

1.  转义，取消特殊字符的作用（`&`、`|`、`<`、`>`、`!` 等，不包括 `%`）
2.  分行写命令：在末尾添加 ^。

# 2. 变量

## 2.1 设置变量

注意：

1.  凡是修改变量的值，都要使用 set 命令。
2.  变量值可以是语句。
3.  使用变量：`%变量名%`。



设置变量：

```bash
set 变量名=变量值
```

接受用户输入：

```bash
set /p 变量名=提示语句
```

保存算数运算的结果：

```bash
set /a 变量名=表达式
```

拼接字符串：

```bash
set 变量=%变量1%%变量2%
```

将命令结果赋给变量：

```bash
for /f %%i in ('命令') do set 变量名=%%i
```

设置环境变量：

```pl-sql
setx /m name "value"
```

```pl-sql
setx -m name "%name%;value"
```

## 2.2 输出扩展

| **修饰符**        | **作用**     |
| -------------- | ---------- |
| ******%0**     | 完整路径       |
| ******%\~0**   | 去掉引号       |
| ******%\~d0**  | 所在盘        |
| ******%\~dp0** | 所处路径（含驱动器） |
| ******%\~n0**  | 文件名        |
| ******%\~x0**  | 扩展名        |
| ******%\~nx0** | 文件名及扩展名    |
| ******%\~f0**  | 文件完整路径     |
| ******%\~a0**  | 文件属性       |
| ******%\~t0**  | 修改时间       |
| ******%\~z0**  | 文件大小       |

## 2.3 系统变量

| **系统变量**                      | **含义**               |
| ----------------------------- | -------------------- |
| ******%Userprofile%**         | 用户文件夹                |
| ******%Userprofile%\Desktop** | 桌面路径                 |
| ******%random%**              | 随机数                  |
| ******%computername%**        | 计算机名                 |
| ******%errorlevel%**          | 上一条命令执行结果，&#xA;成功返回0 |
| ******%date%**                | 系统日期                 |
| ******%time%**                | 系统时间                 |
| ******%cd%**                  | 当前路径                 |

## 2.4 局部变量

1.  setlocal 需要搭配 endlocal 使用。
2.  在此范围内的变量，均为局部变量，无法影响外部变量和环境变量。

```bash
setlocal

命令

endlocal
```

# 3. 输出

输出：

```bash
echo 输出内容

```

输出变量的值：

```bash
echo %变量名%
```

输出空行：

```bash
ehco.
```

输出不换行：

```bash
set /p=%变量名%<nul
```

转义字符：

-   `^`：转义特殊符号。
-   `%%`：转义`%`。

## 3.1 输出重定向

**写入文件：**

将命令结果写入文件（覆盖）：

```bash
echo 输出内容>文件名.txt
```

将命令结果写入文件（追加）：

```bash
echo 输出内容>>文件名.txt

```

将文件的内容作为命令的输入：

```bash
命令 < 文件.txt
```

**写入剪贴板：**

将命令结果写入剪贴板：

```bash
命令 ｜ clip
```

将文件的内容写入剪贴板：

```bash
clip < 文件名.txt
```

**特殊情况：**

变量值是数字的情况：

```bash
echo.%变量%>文件.txt
```

变量值是参数（on/of）：

```bash
echo,%变量%>文件.txt
```

**屏蔽报错：**

> 不是所有的命令都有效。

不显示命令输出：

```pl-sql
命令 > nul
```

不显示报错：

```pl-sql
命令 > nul 2 > nul
```

## 3.2 截取和替换

截取指定长度：

```bash
echo %变量名:~开始位置,截取长度%
```

截取前 5 位字符：

```bash
echo %变量名：~,5%
```

截取第一位至倒数第4位：

```bash
echo %变量名:~,-4%
```

截取时间码：

```bash
echo %time:~6,2%秒
```

替换内容：

> 将 `h` 替换成 `H`。

```bash
echo %var:h=H%

```



# 4. 判断 & 选择

## 4.1 if

用法：

```perl
if 条件 (...) else (...)
```

多行写法：

```perl
if 条件 (
  ...
) else (
  ...
)
```

判断路径是否存在（文件夹末尾加 `\` ）：

```bash
if exist 路径
```

判断路径是否不存在：

```bash
if not exist 路径
```

判断变量是否赋值：

```bash
if defined 变量名
```

判断是否传有参数：

```bash
if "%~1"==""
```

```bash
if exist "%1"
```

判断两个字符串是否相等：

```bash
if ""==""
```

当需要判断的变量中有空格：

```bash
if "%变量%"=="内容"
```

忽略大小写：

```bash
if /i 条件
```

比较运算符：

| **not** | 非运算   |
| ------- | ----- |
| **equ** | 等于    |
| **gtr** | 大于    |
| **lss** | 小于    |
| **geq** | 大于或等于 |
| **leq** | 小于或等于 |
| **neq** | 不等于   |

## 4.2 choice

> 有效选择字符: a-z、A-Z、0-9和 128 到 254 的 ASCII 值。

输入y或n：

```bash
choice
```

自定义输入列表

```bash
choice /c 12
```

隐藏列表项内容：

```bash
choice /n
```

显示提示文本：

```bash
choice /m "提示文本"
```

`%errorlevel%`返回值：

-   choice 输入的第一项返回 1，第二项返回 2。

## 4.3 命令组合符

| **符号**   | **作用**                   |
| -------- | ------------------------ |
| **\|**   | 把第一条命令的结果，作为后面第二条命令的参数使用 |
| **&**    | 一行同时执行两条命令               |
| **&&**   | 前面的命令执行成功后，执行第二条命令       |
| **\|\|** | 前面的命令执行失败后，执行第二条命令       |

## 4.4 fc

比较文件是否相同：

```bash
fc <文件1> <文件2>
```

# 5. 运行 & 调用

bat 文件名不能是关键字，在调用时可能会出现死循环。

## 5.1 start

> start 功能等同于双击运行文件，而不是在命令行中打开。

运行程序（打开文件、路径、网址，使用默认打开方式）：

```pl-sql
start 文件路径
```

在资源管理器打开当前目录：

```pl-sql
start .
```

用指定的程序运行文件：

```pl-sql
start 程序路径 文件路径
```

参数：

| **参数** | **作用**     |
| ------ | ---------- |
| /max   | 以最大化运行     |
| /min   | 以最小化运行     |
| /wait  | 启动程序并等待它终止 |

**特殊情况：**

打开的路径有空格：

```pl-sql
start "" "路径"
```

用指定的程序打开文件：

> 前面的加空引号，可以不等待程序运行完毕。

```pl-sql
start "" "程序路径" "文件路径"

```

设置新窗口的标题：

```pl-sql
start "标题" 路径
```

## 5.2 call

> call 不会中断当前脚本的运行，在调用后返回，继续执行后面的命令。

调用另一个bat文件：

```pl-sql
call *.bat
```

打印变量内容：

```pl-sql
ecoh %变量名%
```

运行变量中的语句：

```pl-sql
call %变量名%
```

## 5.3 标签

> 标签用于定位，与 `call` 和 `goto` 配合使用。

定义标签：

```perl
:标签名

命令

goto :eof
```

`:eof`（End Of File）：

-   `goto :eof` 可以调用后返回，实现编程语言中函数的功能。



调用标签：

> 调用后返回原位置。

```pl-sql
call :标签名
```

向标签传参：

> 标签中用 `%1` 接收。

```pl-sql
 call :标签名 参数1
```

跳转标签：

> 跳转后不返回，在标签所在处继续执行。

```pl-sql
goto :标签名
```

## 5.4 命令行参数

-   `%0` ：bat 文件本身，只输入 %0 可以重复运行
-   `%1` ：参数1，可以拖动文件到 bat 上
-   `%*` ：将所有文件作为参数。

**移除参数：**

1.  bat 能接收的参数有限：%1\~%9。
2.  shift 命令能移除其中一个参数，后面的参数会向前偏移，替换它的位置。

```abap
shift /n
```

例如：

1.  `shift /1`会移除参数1，此时参数2就变成了参数1。
2.  执行完命令行，%1 相当于之前的 %2。

## 5.5 exit

关闭窗口：

```bash
exit
```

退出被调用的bat（不关闭窗口）：

```bash
exit /b
```

# 6. for循环

## 6.1 循环遍历

语法：

> bat 中用`%%i`表示循环变量，cmd中用`%i`表示。

```powershell
for 循环变量 in (遍历范围) do (执行命令)
```

在集合中遍历：

```powershell
for %%i in (1,2,3) do (echo %%i)
```

有序范围遍历：

```powershell
for /l %%i in (起点,步长,终点) do (echo %%i)
```

遍历文件：

```powershell
for /f %%i in (文件.txt) do (echo %%i)
```

```powershell
for %%i in (*.*) do (echo %%i)
```

```powershell
for %%i in (??.txt) do (echo %%i)
```

## 6.2 分割字符

拆分文本（默认保留第一段）：

> 分隔符为字符或字符列表，列表里的内容都会作为分割符

```.properties
for /f “delims=分隔符" ...
```

设置保留部分：

```.properties
for /f "tokens=2 delims=分割符" ...
```

保留多段字符用多个变量接收：

```.properties
for /f "tokens=2,4" %%i in () do (echo %%i %%j)
```

不分割（避免因为空格无法显示整行）：

> 默认有空格分割。

```.properties
for /f "delims=" ...
```

用引号分为分隔符：

```.properties
for /f tokens^=2^ delims^=^" ...
```

去掉行首的空格

```.properties
for /f tokens=* ...
```

## 6.3 延时变量

设置延时变量：

```.properties
setlocal enabledelayedexpansion
```

使用延时变量：

```.properties
!变量名!
```

# 7. 调用 vbs

## 7.1 弹窗

> 提示框样式：16、64

```vb.net
mshta vbscript:msgbox("输出内容",64,"标题")(window.close)
```

## 7.2 获取管理员权限

```vb.net
(PUSHD "%~DP0")&(REG QUERY "HKU\S-1-5-19">NUL 2>&1)||(powershell -Command "Start-Process '%~sdpnx0' -Verb RunAs"&&exit)
cd %~dp0
```

```vb.net
:: 不能传递参数
%1 mshta vbscript:CreateObject("Shell.Application").ShellExecute("cmd.exe","/c %~s0 ::","","runas",1)(window.close)&&exit
```

## 7.3 创建快捷方式

冒号：一个行写多条语句。

```vb.net
set exe_path=*.exe
set lnk_name=*
mshta VBScript:Execute("Set a=CreateObject(""WScript.Shell""):Set b=a.CreateShortcut(a.SpecialFolders(""\"") & ""%lnk_name%.lnk""):b.TargetPath=""%exe_path%"":b.Save:close")
```

参数：

| **参数**           | **含义**           |
| ---------------- | ---------------- |
| Arguments        | 目标程序参数           |
| Description      | 快捷方式备注           |
| FullName         | 返回快捷方式完整路径       |
| Hotkey           | 快捷方式快捷键          |
| IconLocation     | 快捷方式图标，不设则使用默认图标 |
| TargetPath       | 目标               |
| WindowStyle      | 窗口启动状态           |
| WorkingDirectory | 起始位置             |

示例：

```vb.net
set a = WScript.CreateObject("WScript.Shell")
    
b.SpecialFolders("Desktop")                   '获得桌面目录
b.CreateShortcut(strDesktop & "\GzServer.lnk")     '快捷方式存放目录及名称
b.TargetPath = "D:\GbServer\GBServer.bat"       '指向的可执行文件
b.WindowStyle = 1                             '运行方式(窗体打开的方式)
b.oShellLink.Hotkey = "CTRL+SHIFT+F"          '快捷键
b.IconLocation = "D:\GbServer\img\log.ico, 0"     '图标(同样可不指定)
b.Description = "ChinaDforce YanMang"          '备注信息
b.WorkingDirectory = "D:\GbServer\"           '起始目录
b.Save                                        '保存快捷方式
```
