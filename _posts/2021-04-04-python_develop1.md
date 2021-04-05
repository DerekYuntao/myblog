---
layout: post
title: "Python Development - Level1"
date: 2021-04-04
description: Ways to develop a projet/program by python - Level1 fundamental python 
share: true
tags:
 - python
---

## 1 PyCharm IDE
PyCharm is a Python IDE, the integrated functions include：
- Project管理
- 代码提示
- 语法高亮
- 调试
- 解释器
- 框架和库

### create project
    [Create New Project] -- 选择项目根目录和解释器版本 -- [Create]


### create code file
    项目根目录或根目录内部任意位置 — 右键 -- [New] -- [Python File]

### run
    文件打开状态 -- 空白位置 — 右键 -- Run 


### PyCharm settings
    [file] -- [Settings]/[Default Settings]

**修改主题**

    [Appearance & Behavior] -- [Appearance]

- Theme：修改主题
- Name：修改主题字体
- Size：修改主题字号

**修改代码文字**

    [Editor] -- [Font]

- Font：修改字体
- Size：修改字号
- Line Spacing：修改行间距

**修改解释器**

    [Project: name] -- [Project Interpreter] 

**项目管理**
***open project***

    [File] -- [Open] -- 浏览选择目标项目根目录 -- [OK] -- 选择打开项目方式

三种打开方式：

1. This Window 

覆盖当前项目，打开新项目

2. New Window
在新窗口打开，则打开两个PyCharm，每个PyCharm负责一个项目。

3. Attach
一个PyCharm下同时打开两个项目

***close project***

    [File] -- [Close Project]/[Close Projects in current window]

## 2 Comments
1. 单行注释 # {contests}. 快捷键 ctrl+/
如在一行代码后面写**一行简单的注释**，通常打两个空格再写
2. 多行注释 
```python
    """
    a
    b
    c
    """
# or

    '''
    a
    b
    c
    '''
```
 ## 3 variable
 变量是存储数据时,当前数据所在的**内存地址的名字**
 
 **定义变量**
 变量名 = 值

Python标识符命名规则：字母数字下划线组成; 不能数字开头; 不使用内置关键字; 区分大小写
关键字：
```html
False     None    True   and      as       assert   break     class  
continue  def     del    elif     else     except   finally   for
from      global  if     import   in       is       lambda    nonlocal
not       or      pass   raise    return   try      while     with  
yield
```

变量命名习惯：
每个单词首字母大写: ModelCase
第一个单词首字母小写：modelCase
下划线：model_case

## 4 Debug
Debug tool是IDE中集成的用来调试程序的工具，在这里程序员可以查看程序的执行细节和流程或者调试bug

Debug步骤：
1. 打断点
- 断点位置
目标要调试的代码块的第一行代码，即一个断点即可(方式：单击行号右边空白)
- 打断点的方法
单击目标代码的行号右侧空白位置

2. Debug调试
打断点后，在文件内部任意位置右键 -- Debug'file name' — 调出Debug工具面板 -- 单击Step Over/F8，即可按步执行代码


Debug输出面板:
- Debugger
  - 显示变量的具体信息
- Console
  - 输出内容

## 5 Data type
**int**
num1 = 1
print(type(num1))

**float**
num2 = 2.1
print(type(num2))

**str**
a = 'hello world'
print(type(a))

**bool : True/False**
b = True
print(type(b))

**list**
c = [10, 20, 30]
print(type(c))

**tuple** 
d = (10, 20, 30)
print(type(d))

**set** 
e = {10, 20, 30}
print(type(e))

**dict -- key and value**
f = {'name': 'TOM', 'age': 18}
print(type(f))

## 5 Print
格式化符号

| 格式符号 |          转换          |
| :------: | :--------------------: |
|  %s      |         字符串         |
|  %d      |   有符号的十进制整数   |
|  %f      |         浮点数         |
|    %c    |          字符          |
|    %u    |    无符号十进制整数    |
|    %o    |       八进制整数       |
|    %x    | 十六进制整数（小写ox） |
|    %X    | 十六进制整数（大写OX） |
|    %e    | 科学计数法（小写'e'）  |
|    %E    | 科学计数法（大写'E'）  |
|    %g    |      %f和%e的简写      |
|    %G    |      %f和%E的简写      |

- %06d，表示输出的整数显示位数，不足位数的左侧以0补全，超出当前位数则以原样的数据输出
- %.2f，表示小数点后显示的小数位数

**格式化字符串除了%s，在3.6+的python版本还可以写为更简单的`f'{表达式}'`**
e.g.

```python
age = 18 
name = 'TOM'
weight = 75.5
student_id = 1

# My ID is 0001
print('My ID is %4d' % student_id)

# My name is Tom and will be 19 next year
print('My name is%s and will be%d next year' % (name, age + 1))

# 强大的%s: My name is Tom and will be 19 next year
print('My name is%s and will be%s next year' % (name, age + 1))

# 高效的f{}: My name is Tom and will be 19 next year
print(f'My name is{name} and will be{age + 1} next year')
```

**结束符**
两个print会换行输出,说明print隐含了以'\n'作为默认结束符，用户可以按需更改结束符

```python
print('Hellow world', end="\n")

print('Hellow world', end="...")
print('Hellow earth', end="\n")
```

## 6 Input
特点：
- Python中程序执行到`input`，等待用户输入，输入完成之后继续执行
- `input`接收用户输入后，一般存储到一个字符串变量，方便后续使用
- `input`会把接收到的输入的数据都当做字符串处理
e.g.

```python
password = input('Entering your password：')
print(f'Your password is: {password}')
# <class 'str'>
print(type(password))
```

## 7 Data type conversion

|          函数          |                        说明                         |
| :--------------------: | :-------------------------------------------------: |
|  ==int(x [,base ])==   |                  将x转换为一个整数                  |
|     ==float(x )==      |                 将x转换为一个浮点数                 |
| complex(real [,imag ]) |        创建一个复数，real为实部，imag为虚部         |
|      ==str(x )==       |                将对象 x 转换为字符串                |
|        repr(x )        |             将对象 x 转换为表达式字符串             |
|     ==eval(str )==     | 用来计算在字符串中的有效Python表达式,并返回一个对象 |
|     ==tuple(s )==      |               将序列 s 转换为一个元组               |
|      ==list(s )==      |               将序列 s 转换为一个列表               |
|        chr(x )         |           将一个整数转换为一个Unicode字符           |
|        ord(x )         |           将一个字符转换为它的ASCII整数值           |
|        hex(x )         |         将一个整数转换为一个十六进制字符串          |
|        oct(x )         |          将一个整数转换为一个八进制字符串           |
|        bin(x )         |          将一个整数转换为一个二进制字符串           |

e.g.

``` python
# to list
t1 = (100, 200, 300)
print(list(t1))
print(type(list(t1)))

# eval() -- 将字符串中的数据转换成Python表达式原本类型
str1 = '10'
str2 = '[1, 2, 3]'
str3 = '(1000, 2000, 3000)'
print(type(eval(str1)))  #int
print(type(eval(str2)))  #list
print(type(eval(str3)))  #tuple
```

## 8 Operator
运算符分类:
- 算数运算符

| 运算符 |  描述  | 实例                                               |
| :----: | :----: | :------------------------------------------------:|
|   +    |   加   | 1 + 1 输出 2                                    |
|   -    |   减   | 1-1 输出 0                                      |
|   *    |   乘   | 2 * 2 输出 4                                    |
|   /    |   除   | 10 / 2 输出 5                                   |
|   //   |  整除  | 9 // 4 输出2                                    |
|   %    |  取余  | 9 % 4 输出 1                                    |
|   **   |  指数  | 2 ** 4 输出 16                                  |
|   ()   | 小括号 | 小括号提高运算优先级，即 (1 + 2) * 3 输出结果为 9 |

**混合运算优先级顺序：`()`高于 `**` 高于 `*` `/` `//` `%` 高于 `+` `-`

- 赋值运算符`=`
将`=`右侧的结果赋值给左侧的变量 
多变量赋值:
e.g.

```python
num1, num2, str1 = 10, 0.5, 'hello world'
print(num1)
print(num2)
print(str1)
#多变量赋相同值
a = b = 10
```

- 复合赋值运算符

| 运算符 |  实例                       |
| :------: | :--------------------------:|
| +=     | c += a 等价于 c = c + a    |
| -=     | c -= a 等价于 c = c- a     |
| *=     | c *= a 等价于 c = c * a    |
| /=     | c /= a 等价于 c = c / a    |
| //=    | c //= a 等价于 c = c // a  |
| %=     | c %= a 等价于 c = c % a    |
| **=    | c ** = a 等价于 c = c ** a |

 优先级:
 1. 先算复合赋值运算符右侧的表达式
 2. 再算复合赋值运算的算数运算
 3. 最后算赋值运算

e.g.

```python
a = 100
a += 1
# a = 100 + 1 = 101
print(a)

c = 10
c *= 1 + 2
# c *= 3 then c=c*3=30
print(c)
```

- 比较运算符
比较运算符也称为关系运算符，通常用于判断。

| 运算符| 描述| 
| ------------ | ------------------------------------------------------- |
| ==     | 判断相等。如果两个操作数的结果相等，则条件结果为真(True)，否则条件结果为假(False) | 
| !=     | 不等于。如果两个操作数的结果不相等，则条件为真(True)，否则条件结果为假(False) |
| >      | 运算符左侧操作数结果是否大于右侧操作数结果，如果大于，则条件为真，否则为假 |  
| <      | 运算符左侧操作数结果是否小于右侧操作数结果，如果小于，则条件为真，否则为假 |
| >=     | 运算符左侧操作数结果是否大于等于右侧操作数结果，如果大于，则条件为真，否则为假 | 
| <=     | 运算符左侧操作数结果是否小于等于右侧操作数结果，如果小于，则条件为真，否则为假 |  

- 逻辑运算符

| 运算符 | 逻辑表达式 | 描述                                                         | 实例                                     |
| ------ | ---------- | ------------------------------------------------------------ | ---------------------------------------- |
| and    | x and y    | Bool "与"：如 x 为 False，x and y 返回 False，否则它返回 y 的值 | True and False， 返回 False            |
| or     | x or y     | Bool "或"：如 x 是 True，它返回 True，否则它返回 y 的值   | False or True， 返回 True              |
| not    | not x      | Bool "非"：如 x 为 True，返回 False; 如果 x 为 False，返回 True | not True 返回 False, not False 返回 True |

e.g.
```python
a, b, c = 1, 2, 3
print((a < b) and (b < c))  # True
print(not (a > b))          # True
```
e.g. **易错：数字之间的逻辑运算**

```python
a = 0
b = 1
c = 2

# and运算符，只要有一个值为0，则结果为0，否则结果为最后一个非0数字
print(a and b)  # 0
print(c and a)  # 0
print(b and c)  # 2
print(c and b)  # 1

# or运算符，只有所有值为0结果才为0，否则结果为第一个非0数字
print(a or b)  # 1
print(a or c)  # 2
print(b or c)  # 1
```

