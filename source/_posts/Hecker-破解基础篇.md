---
title: Hecker-破解基础篇
tags:
  - 破解
  - OLLYDBG
categories:
  - IT
  - Hecker
abbrlink: 6e06fdf9
date: 2021-09-07 18:11:57
---

### OKKYDBG 常用快捷键
```
F2 设置断点 只要在光标定位的位置按F2键即可 再按一次F2键则会删除断点
F8 单步步过 每按一次F8执行一条反汇编窗口中的一条指令 遇到call等子程序不进入其代码
F7 单步步入 功能单步步过F8类似 区别是遇到call等子程序会进入其中 进入后首先会停留在子程序的第一条指令上
F4 运行到选定位置 作用是直接运行到光标所在位置处暂停
F9 运行 按下这个键如果没有设置相应断电的话 被调试的程序将直接开始运行
Ctrl+F9 执行到返回 此命令在执行到一个ret返回指令时暂停 常用于从系统部分返回到我们调试的程序部分
Alt+F9 执行到用户代码
```

### F2断点 CC断点
```
1 替换指令 也就是替换int3指令
2 od检测到int3指令之后会引发一个异常并且捕获它 这时候程序就会中断
3 int3指令给删除掉 还原之前的代码
```

### OLLYDBG 寄存器
```
C CF (Carry Flag) 进位标志位 无符号运算的结果超过寄存器存放的最大值 C=1 没有超过 C=0
P PF (Parity Flag) 奇偶标志位 偶数为1 奇数为0
A AF (Auxiliary Carry Flag) 辅助进位标志位
Z ZF (Zero Flag) 零标志位 如果一条语句的计算结果是0 Z=1 不是0 Z=0
S SF (Sign Flag) 符号标志位 如过指令运算结果是负数 S=1 反之 S=0
T TF (Trace Flag) 定时器溢出标志位
O OF (Overflow Flag) 溢出标志位 一个寄存器如果存放的值超过所能表示的范围 就称为溢出 O溢出时被置为1 否则 O的值被清为0
D DF (Direction Flag) 方向标志位
```

### 汇编语法
```
TEST 逻辑运算指令 测试(两操作数作与运算 仅修改标志位 不回送结果)
TEST 对两个参数(目标,源)执行AND逻辑操作,并根据结果设置标志寄存器,结果本身不会保存
(TEST AX,BX) 与 (AND AX,BX) 命令有相同效果 只是Test指令不改变AX和BX的内容,而AND指令会把结果保存到AX中

JE 表示等于就跳转
JNE 不等于就跳转

JZ 为零则跳转
JNZ 不为零则跳转
```

### Win32API
```
GetDlgItemText 获得与对话框中的控件相关的标题或文本
GetDlgItemTextA ASCII版本的函数
GetDlgItemTextW Unicode版本的函数

HWND dlgItem = GetDlgItem(编辑框ID)
dlgItem->GetWindowText 获得编辑框的内容

MessageBox 显示消息框
MessageBoxA ASCII版本的函数
MessageBoxW Unicode版本的函数

EnableWindow 允许/禁止指定的窗口或控件接受鼠标和键盘的输入 当输入被禁止时 窗口不响应鼠标和按键的输入 输入允许时 窗口接受所有的输入

RegQueryValueEx RegQueryValue 检索与打开的注册表项相关联的指定值名称的类型和数据
RegQueryValueExA RegQueryValueA ASCII版本的函数
RegQueryValueExW RegQueryValueW Unicode版本的函数

RegOpenKey RegOpenKeyEx 打开指定的注册表项
RegOpenKeyA RegOpenKeyExA ASCII版本的函数
RegOpenKeyW RegOpenKeyExW Unicode版本的函数
```

### 破解步骤
```
1. 先用PEID查壳
2. 用DIE64查一下编程语言
3. 先脱壳再分析
4. 打开OD 搜索关键字进行破解
5. 修改完成后 右键复制到可执行文件 所有修改 选择保存文件

PS: 未脱壳程序直接使用OD破解
1. 按F9把程序运行起来，然后在代码区按CTRL+G转到00401000这个位置 再去搜索
2. 有壳的程序修改代码以后无法保存 使用内存补丁工具Inline Patch生成器1.2
3. 打开Inline Patch 选择软件的路径 输入不定地址以及补丁代码 点击添加 生成
```

### 内存访问破解
```
1. 打开内存窗口 搜索关键字
2. 打开cpu窗口 在数据窗口转到关键字所在的内存
3. 选择关键字 右键断点 内存访问
4. 执行程序 OD断点触发后 删除内存断点
5. 当前断点位置 进行破解
```

### 易语言技巧
```
过二进制搜索 FF 25 来到这一群JMP的位置 找到PUSH 这是易语言特征 打开的第一个窗口的ID 改变该ID就可以直接打开子窗口

PUSH 52012C8E
弹出新窗口的CALL上面会有入栈代码 2C8E 表示窗口ID

PUSH 10001
子窗口ID的代码上面会有一个PUSH 10001 这是易语言通用的 每个窗口ID语句上面都会有一个PUSH 10001
遍历所有
```
