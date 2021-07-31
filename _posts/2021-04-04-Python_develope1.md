---
layout: post
title: "Python Development - Level1"
date: 2021-04-04
description: Ways to develop a projet/program by python - Level1 fundamental python 
share: true
tags:
 - python
---

<!-- vscode-markdown-toc -->
* 1. [1 PyCharm IDE](#PyCharmIDE)
	* 1.1. [create project](#createproject)
	* 1.2. [create code file](#createcodefile)
	* 1.3. [run](#run)
	* 1.4. [PyCharm settings](#PyCharmsettings)
* 2. [2 Comments](#Comments)
* 3. [3 variable](#variable)
* 4. [4 Debug](#Debug)
* 5. [5 Data type](#Datatype)
* 6. [6 Print](#Print)
* 7. [7 Input](#Input)
* 8. [8 Data type conversion](#Datatypeconversion)
* 9. [9 Operator](#Operator)
	* 9.1. [算数运算符](#)
	* 9.2. [复合赋值运算符](#-1)
	* 9.3. [比较运算符](#-1)
* 10. [10 IF statement](#IFstatement)
* 11. [11 Loops](#Loops)
	* 11.1. [**while**](#while)
	* 11.2. [**for**](#for)
	* 11.3. [**Exit loop**](#Exitloop)
	* 11.4. [**循环和else的配合使用**](#else)
* 12. [12 String](#String)
* 13. [13 Container](#Container)
	* 13.1. [**List**](#List)
	* 13.2. [**Tuple**](#Tuple)
	* 13.3. [**Dictionary**](#Dictionary)
	* 13.4. [**Set**](#Set)
* 14. [14 Advanced feature for containers](#Advancedfeatureforcontainers)
	* 14.1. [**Iteration 迭代**](#Iteration)
	* 14.2. [**Comprehensions 生成式/推导式**](#Comprehensions)
	* 14.3. [**生成器**](#-1)
	* 14.4. [**迭代器**](#-1)
	* 14.5. [**Public method**](#Publicmethod)
	* 14.6. [**Public operations**](#Publicoperations)
* 15. [15 python function](#pythonfunction)
	* 15.1. [**函数说明文档**](#-1)
	* 15.2. [**作用域**](#-1)
		* 15.2.1. [**多个返回值**](#-1)
	* 15.3. [**参数传递**](#-1)
	* 15.4. [**引用**](#-1)
	* 15.5. [**高级特性**](#-1)
	* 15.6. [**lambda function 匿名函数**](#lambdafunction)
	* 15.7. [**高阶函数**](#-1)
	* 15.8. [**装饰器**](#-1)
	* 15.9. [**偏函数**](#-1)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->

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
Debug tool是IDE中集成的用来调试程序的工具，在这里可以查看程序的执行细节和流程或者调试bug

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
```python
#int
num1 = 1
print(type(num1))

#float
num2 = 2.1
print(type(num2))

#str
a = 'hello world'
print(type(a))

#bool : True/False
b = True
print(type(b))

#list
c = [10, 20, 30]
print(type(c))

#tuple
d = (10, 20, 30)
print(type(d))

#set
e = {10, 20, 30}
print(type(e))

#dict -- key and value
f = {'name': 'TOM', 'age': 18}
print(type(f))
```

## 6 Print
格式化符号

| 格式符号 |          转换          |
| :------: | :--------------------: |
|    %s    |         字符串         |
|    %d    |   有符号的十进制整数    |
|    %f    |         浮点数         |
|    %c    |          字符          |
|    %u    |    无符号十进制整数    |
|    %o    |       八进制整数       |
|    %x    | 十六进制整数（小写ox） |
|    %X    | 十六进制整数（大写OX） |
|    %e    | 科学计数法（小写'e'）  |
|    %E    | 科学计数法（大写'E'）  |
|    %g    |      %f和%e的简写      |
|    %G    |      %f和%E的简写      |
|    %%    |         百分数        |

- %06d 表示输出的整数显示位数，不足位数的左侧以0补全，超出当前位数则以原样的数据输出
- %.2f 表示小数点后显示的小数位数

**格式化字符串除了%s，在3.6+的python版本还可以写为更简单的`f'{表达式}'`**
e.g.

```python
age = 18 
name = 'TOM'
weight = 75.5
student_id = 1

# My ID is 0001
print('My ID is %4d' %(student_id))

# My name is Tom and will be 19 next year
print('My name is %s and will be %d next year' %(name, age + 1))

# 强大的%s: My name is Tom and will be 19 next year
print('My name is %s and will be %s next year' %(name, age + 1))

# 高效的f{}: My name is Tom and will be 19 next year
print(f'My name is {name} and will be {age + 1} next year')

# print a value in percentage
value = 0.20009
# precp_MWF:20.0%
print("precp_MWF" + ":" + " %.1f%%" %(value*100))
```

**结束符**
两个print会换行输出,说明print隐含了以'\n'作为默认结束符，用户可以按需更改结束符

```python
print('Hellow world', end="\n")
print('Hellow world', end="...")
```

## 7 Input
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

## 8 Data type conversion

|          函数          |                        说明                         |
| :--------------------: | :-------------------------------------------------: |
|  int(x [,base ])   |                  将x转换为一个整数                  |
|     float(x)      |                 将x转换为一个浮点数                 |
| complex(real [,imag ]) |        创建一个复数，real为实部，imag为虚部         |
|      str(x)       |                将对象 x 转换为字符串                |
|        repr(x)        |             将对象 x 转换为表达式字符串             |
|    eval(str)     | 用来计算在字符串中的有效Python表达式,并返回一个对象 |
|     tuple(s)      |               将序列 s 转换为一个元组               |
|      list(s)      |               将序列 s 转换为一个列表               |
|        chr(x)         |           将一个整数转换为一个Unicode字符           |
|        ord(x)         |           将一个字符转换为它的ASCII整数值           |
|        hex(x)         |         将一个整数转换为一个十六进制字符串          |
|        oct(x)         |          将一个整数转换为一个八进制字符串           |
|        bin(x)         |          将一个整数转换为一个二进制字符串           |

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

## 9 Operator
运算符分类:
### 算数运算符

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

### 复合赋值运算符

| 运算符 |  实例                       |
| :------: | :--------------------------:|
| +=     | c += a is c = c + a    |
| -=     | c -= a is c = c- a     |
| *=     | c *= a is c = c * a    |
| /=     | c /= a is c = c / a    |
| //=    | c //= a is c = c // a  |
| %=     | c %= a is c = c % a    |
| **=    | c ** = a is c = c ** a |

 优先级:
 1. 先算复合赋值运算符右侧的表达式
 2. 再算复合赋值运算的算数运算
 3. 最后算赋值运算

e.g.

```python
a = 100
a += 1
# a = a + 1 = 101
print(a)

c = 10
c *= 1 + 2
# c *= 3 then c=c*3=30
print(c)
```

### 比较运算符
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

## 10 IF statement
e.g. random mora

```python
import random
computer = random.randint(0, 2)   #0,1,2
#print(computer)

player = int(input('Please punch：0-stone，1-scissors，2-cloth'))

# player win: p0:c1 or p1:c2 or p2:c0
if (player == 0 and computer == 1) or (player == 1 and computer == 2) or (player == 2 and computer == 0):
    print('congradulations, you are the winner!')

# draw
elif player == computer:
    print('draw')
else:
    print('Computer win!')
```

**三目运算符(三元运算符)**
化简简单的一句if-else语句

`条件成立执行的表达式 if 条件 else 条件不成立执行的表达式`

``` python
a = 1
b = 2

c = a if a > b else b
print(c)  #c=2
```

## 11 Loops
### **while**
    while 条件
        条件成立执行的代码
e.g. accumulation of even numbers

```python
#way1(prefer): 条件判断和2取余数为0则累加计算 
i = 1
result = 0
while i <= 100:
    if i % 2 == 0:
        result += i
    i += 1

print(result)  # print 2550

# Way2：counter control 计数器累加2
i = 0
result = 0
while i <= 100:
    result += i
    i += 2

print(result)  # print 2550
```

### **for**
    for 临时变量 in 序列：
        重复执行的代码

### **Exit loop**
- break: terminate the whole loop
- continue: exit the current loop and continue to execute the next loop code

e.g. print stars

需求为：一行打印5个星号，重复5行

``` html
*****
*****
*****
*****
*****
```

``` python
# 重复打印5行
j = 0
while j <= 4:
    # 一行打印
    i = 0
    while i <= 4:
        # 一行内的星星不能换行，取消print默认结束符\n
        print('*', end='')
        i += 1
    # 每行结束要换行，这里借助一个空的print，利用print默认结束符换行
    print()  #相当于print('\n')
    j += 1
```

e.g.打印九九乘法表

``` python
j = 1
while j <= 9:
    i = 1
    while i <= j:
        print(f'{i}*{j}={i*j}', end=' ')
        i += 1
    print()  #or print('\n')
    j += 1
```
result:

```html
1*1=1 
1*2=2 2*2=4 
1*3=3 2*3=6 3*3=9 
1*4=4 2*4=8 3*4=12 4*4=16 
1*5=5 2*5=10 3*5=15 4*5=20 5*5=25 
1*6=6 2*6=12 3*6=18 4*6=24 5*6=30 6*6=36 
1*7=7 2*7=14 3*7=21 4*7=28 5*7=35 6*7=42 7*7=49 
1*8=8 2*8=16 3*8=24 4*8=32 5*8=40 6*8=48 7*8=56 8*8=64 
1*9=9 2*9=18 3*9=27 4*9=36 5*9=45 6*9=54 7*9=63 8*9=72 9*9=81  
```

### **循环和else的配合使用**
循环可与else配合使用, else下⽅方缩进的代码是当循环正常结束之后要执⾏的代码，否则这部分代码不执行

    while 条件:
        条件成⽴重复执⾏的代码
    else:
        循环正常结束后要执⾏的代码

    for 临时变量量 in 序列:
        重复执⾏的代码
    else:
        循环正常结束后要执⾏的代码

e.g.

```python
str1 = 'hereiam'
for i in str1:
    if i == 'a':
        print('Do not print a！循环非正常结束，不再打印后续')
        break
    print(i)
else:
    print('循环正常结束，已全部打印')
```
以上代码输出：
herei
Do not print a！循环非正常结束，不再打印后续。

因此该code没有执行else之后的代码块

## 12 String
- 三引号字符串
三引号字符串支持换行

``` python
a = ''' This is my blog, 
        welcome!
        I \'m waiting for you!'''
print(a)
```        

切⽚是对操作对象截取其中⼀部分的操作,字符串、列表、元组都支持切⽚操作.
start:end:interval

e.g. 

```python
name = "abcdefg"
print(name[2:5:1]) # cde
print(name[::2]) # aceg
print(name[:-1]) # abcdef
print(name[-1]) #g
print(name[-4:-1]) # def
print(name[::-1]) # gfedcba #reverse
```

- Find string
    string.find(⼦串, 开始下标, 结束下标)
检测某个⼦串是否包含在字符串中，是则返回此⼦串开始的位置下标，否则返回-1    

rfind()：与find()功能相同，但查找⽅方向为右侧开始
rindex()：与index()功能相同，但查找⽅方向为右侧开始
count()：返回某个⼦串在字符串中出现的次数
- index 类似
e.g. 

```python
mystr = "Welcome to this blog and have a good time and enjoy!"
print(mystr.index('and'))  # 21
print(mystr.index('and', 25, 50))  # 42
print(mystr.count('and'))  # 2
```

- Replace string
    string.replace(旧⼦串, 新⼦串, 替换次数)
替换次数: 获得⼦串出现次数后，替换次数为该⼦串出现的次数 
e.g. 

```python
mystr = "Welcome to this blog and have a good time and enjoy!"
print(mystr.replace("and", "to"))  # Welcome to this blog to have a good time to enjoy!
print(mystr.replace("and", "to", 10))  # Welcome to this blog to have a good time to enjoy!
repstr = mystr.replace("and","to", 1)
print(repstr)  # Welcome to this blog to have a good time and enjoy!
print(mystr)   # Welcome to this blog and have a good time and enjoy!
```
*重要：上例中字符串本身并没有被修改，除非赋值给一个新的变量，这说明**字符串是不可变的数据类型**

- Split string
    string.split(分割字符, num)
num表示分割字符出现的次数，即将来返回num+1个元素组成的list
e.g.

```python
mystr = "Welcome to this blog and have a good time and enjoy and python!"
print(mystr.split("and"))  # ['Welcome to this blog ', ' have a good time ', ' enjoy ', ' python!']
print(mystr.split("and", 2))  # ['Welcome to this blog ', ' have a good time ', ' enjoy and python!']
```

- Join string
    分隔符.join(多个字符/串组成的序列)

e.g.

```python
mychar = ['I', 'like', 'programming']
print(' '.join(mychar))  # I like programming
print('...'.join(mychar))  # I...like...programming
```

- Case conversion
**首字母大写**
    string.capitalize()

```python
mystr = "welcome to this blog and have a good time and enjoy and python!"
print(mystr.capitalize())  # Welcome to this blog and have a good time and enjoy and python!
```

**每个单词首字母大写**
    string.title()

```python
mystr = "welcome to this blog and have a good time and enjoy and python!"
print(mystr.title())  # Welcome To This Blog And Have A Good Time And Enjoy And Python!
```

**所有字符大小写转换**
    string.lower()：将字符串中⼤写转⼩写
    string.upper()：将字符串中小写转大写

**空白字符处理**
    string.lstrip()：删除字符串左侧空⽩字符
    string.rstrip()：删除字符串右侧空⽩字符
    string.strip()：删除字符串两侧空⽩字符
    string.ljust(⻓度, 填充字符)：返回⼀个原字符串向左对齐,并使⽤指定字符(默认空格)填充⾄至对应长度的新字符串
    string.rjust(⻓度, 填充字符)：返回⼀个原字符串向右对齐,并使⽤指定字符(默认空格)填充⾄至对应长度的新字符串
    string.center(⻓度, 填充字符)：返回⼀个原字符串居中对齐,并使⽤指定字符(默认空格)填充⾄至对应长度的新字符串

e.g.

```python
mystr = "Welcome to this blog!"
print(mystr.ljust(30, '_'))  # Welcome to this blog!_________
print(mystr.rjust(30, '_'))  # _________Welcome to this blog!
print(mystr.center(30, '_'))  # ____Welcome to this blog!_____
```

## 13 Container
### **List**
- 判断元素是否存在: in/not in
e.g.

```python
alist = ['a', 'b', 'c']
print('a' in alist)  # True
print('d' not in alist)  # True
```

- 追加
```python
alist = ['a', 'b', 'c']
alist.append(['d', 'e'])   # 追加整个序列到列表
print(alist)  # ['a', 'b', 'c', ['d', 'e']]

alist.extend(['d', 'e'])  # 追加序列元素到列表
print(alist)  # ['a', 'b', 'c', ['d', 'e'], 'd', 'e']

alist.insert(2, 'd')  
print(alist)  # ['a', 'b', 'd', 'c', ['d', 'e'], 'd', 'e']
```

- 删除
    del 目标
    string.pop(): 删除指定索引的数据，默认为最后一个，注意这里返回的是该删除的数据
    string.remove(数据)：移除列表中某个指定数据的第一个匹配项
    string.clear(): 清空列表 

e.g.

```python
alist = ['a', 'b', 'c']
del alist[0]
print(alist)  # ['b', 'c']

del alist  # no alist anymore 

alist = ['a', 'b', 'c']  
alist_del = alist.pop(0)
print(alist_del)  # a
print(alist)  # ['b', 'c']

alist.remove('b')
print(alist)  # ['c']

alist.clear()
print(alist)  # []
```

- 修改
    string.reverse(): 逆置
    string.sort(key=None, reverse=False): 排序：reverse = True 降序， reverse = False 升序

e.g.

```python
nlist = [1, 5, 2, 3, 6, 8]
nlist.sort()
print(nlist)  # [1, 2, 3, 5, 6, 8]
```

- 列表遍历
e.g.

```python
    alist = ['a', 'b', 'c']

    i = 0
    while i < len(alist):
        print(alist[i])
        i += 1

# or 
    for i in alist:
        print(i)
```      

### **Tuple**
⼀个元组可以像一个列表那样存储多个数据，数据可以是不同的数据类型，但是元组内的数据元素是不能修改的, 因此元组数据只⽀持查找和访问。
但是如果元组⾥⾯有列表，那么修改元组中列表元素⾥⾯的数据则是可以的。
如果定义的元组只有⼀个数据，那么这个数据后⾯必须添加一个逗号，否则数据类型为这个唯一的数据元素数据类型。

e.g.

```python
num1 = (100,)
print(type(num1)) # tuple
num2 = (100)
print(type(num2)) # int
```

### **Dictionary**
字典⾥的数据是以键值对形式出现，字典数据和数据顺序没有关系，即字典不⽀持下标，后期⽆论数据如何变化，只需要按照对应的键的名字就可以查找到数据。
- 字典的遍历
e.g.

```python
dlist = {}  # null dictionary
dlist = {'name': 'Lucky', 'age': 2, 'gender': 'male'}
for key in dlist.keys():
    print(key)

for item in dlist.items():
    print(item)

# important
for key, value in dlist.items():
    print(f'{key} = {value}')
```

### **Set**
集合可以去掉重复数据, 且无序不支持下标索引。
由于集合有去重功能，如果向集合内追加(add)的数据是当前集合已有数据，则不进行任何操作。
update() 追加的数据是序列。
创建集合使⽤用{} 或set(), 但是如果创建空集合只能用set(),因为{}只能创建空字典。

remove() 删除集合中的指定数据，如果数据不存在则报错
discard() 删除集合中的指定数据，如果数据不存在也不会报错
pop() **随机删除**集合中的某个数据，并返回这个数据

e.g.
```python
a1 = set()
print(type(a1)) # set
a2 = {}
print(type(a2)) # dict

a3 = {10, 20, 100}
print(a3)  # {100, 10, 20} 不是{10, 20, 100}, 这体现了集合的无序性
a3.add(100)
print(a3)  # {100, 10, 20}
a3.add('abc')
print(a3) # {100, 10, 20, 'abc'}
a3.update([100, 200, 'ab'])
print(a3) # {100, 200, 10, 'ab', 'abc', 20}
a3.update('abc')
print(a3) # {'abc', 'a', 100, 'b', 200, 10, 'ab', 'c', 20}
a3.remove('ab')
print(a3) # {'abc', 'b', 100, 200, 10, 'a', 20, 'c'}
idel = a3.pop()
print(idel) # abc
print(a3) # {'b', 100, 200, 10, 'a', 20, 'c'}
```

## 14 Advanced feature for containers
### **Iteration 迭代**
给定一个容器对象，可以通过for loop来遍历，这种遍历称为迭代。即迭代是访问集合元素的一种方式。str, list, tuple, dict, set都是可迭代对象。
list这种数据类型有下标固然可以迭代，很多其他数据类型没有下标（e.g. dict and set），但只要是可迭代对象，无论有无下标都可以迭代。
dict不是按照顺序存储的，因此迭代出的结果顺序很可能不一样。默认情况下，dict迭代的是key。如果要迭代value，可以用for value in dict.values()，如果要同时迭代key和value，可以用for k, v in d.items()。
通过collections.abc模块的Iterable类型可判断一个对象是否是可迭代对象。
e.g.
```python
from collections.abc import Iterable
print(isinstance('abc', Iterable))  # True
print(isinstance([1,2,3], Iterable))  # True
print(isinstance({'name': 'Jack', 'age': 20}, Iterable))  # True
print(isinstance(123, Iterable))  # False
```
e.g. 同时引用两个变量的迭代
```python
for x, y in [(1, 1), (2, 4), (3, 9)]:
    print(x, y)
```    

### **Comprehensions 生成式/推导式**
推导式可简化代码

- 列列表推导式
[xx for xx in range()]
- 字典推导式
{xx1: xx2 for ... in ...}
- 集合推导式
{xx for xx in ...}

e.g.
```python
list1 = [i for i in range(10)]
print(list1)  # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

# 创建0-10的偶数列表
list1 = [i for i in range(10) if i % 2 == 0]
print(list1)  # [0, 2, 4, 6, 8]

# 创建如下列表：[(1, 0), (1, 1), (1, 2), (2, 0), (2, 1), (2, 2)]
list1 = [(i, j) for i in range(1, 3) for j in range(3)]
print(list1)

# 创建字典
dict1 = {i: i**2 for i in range(1, 5)}
print(dict1)  # {1: 1, 2: 4, 3: 9, 4: 16}

# 合并字典
list1 = ['name', 'age', 'gender']
list2 = ['Jack', 20, 'man']
dict1 = {list1[i]: list2[i] for i in range(len(list1))}
print(dict1)  # {'name': 'Jack', 'age': 20, 'gender': 'man'}

# 提取数据 (大于等于200的字典数据)
counts = {'MBP': 268, 'HP': 125, 'DELL': 201, 'Lenovo': 199, 'acer': 99}
result = {key: value for key, value in counts.items() if value >= 200}
print(result) # {'MBP': 268, 'DELL': 201}
```

### **生成器**

### **迭代器**

### **Public method**
len(); del or del(); max(); min(); range(start,end,step)  **range()生成的序列不包含end
enumerate() 用于将一个可遍历的数据对象(列表、元组或字符串)组合为一个索引序列，同时列出数据元素和下标，一般与 for 循环搭配使用

e.g.
```python
list1 = ['a', 'b', 'c', 'd', 'e']
for i in enumerate(list1):
    print(i)   # (0, 'a') (1, 'b') (2, 'c')
for idx, char in enumerate(list1, start=1):
    print(f'Index: {idx}; element: {char}') 
    # Index: 1; element: a 
    # Index: 2; element: b
    # Index: 3; element: c
```

### **Public operations**

| Operator |  Operation                     |  Supported container   |
| :------: | :--------------------------:| :--------------------------: |
| +     | merge    | string; list; tumple |
| *     | copy    | string; list; tumple |
| in/not in     | whether elements exist  | string; list; tumple; dict; set |

e.g.
```python
# add
# string
str1 = 'a'
str2 = 'b'
str3 = str1 + str2
print(str3) # ab
# list
list1 = [1, 2]
list2 = [3, 4]
list3 = list1 + list2
print(list3) # [1, 2, 3, 4]
# tumple
t1 = (1, 2)
t2 = (3, 4)
t3 = t1 + t2
print(t3) # (1, 2, 3, 4)

# copy
print('/'*10) # //////////
list1 = [10]
print(list1*10) # [10, 10, 10, 10, 10, 10, 10, 10, 10, 10]
tup = ('Hi',)
print(tup*3) # ('Hi', 'Hi', 'Hi')
```

## 15 python function 
函数是将一段具有独立功能的代码块**封装**起来并命名，调用这个名称即可完成对应的需求。
在开发过程中，函数可以实现高效的代码重用。

```python
def function name(parameters):
    code block
```    

**python函数必须先定义后使用。根据需求，输入参数可有可无**

形参：函数定义时书写的输入参数(非真实数据)
实参：函数调⽤用时书写的输入参数(真实数据)

### **函数说明文档**
```python
def function name(parameters):
    """ 函数说明文档 """
    code block
```    

查看函数说明文档
help(function name)    

e.g. print 5 lines
```python
def print_line():
    print('-' * 20)

def print_lines(num):
    i = 0
    while i < num:
        print_line()
        i += 1

print_lines(5)
```    

### **作用域**
变量作用域是变量生效的范围，包括局部变量和全局变量。
局部变量：定义在函数体内部且仅在函数内部生效的变量。函数内部临时保存数据，当函数调用完成后，销毁局部变量。
全局变量：在函数体内外都能生效的变量。

在实际开发中，一个程序常由多个函数组成，并且多个函数共享某些数据。那么如何在函数体内部修改全局变量？
Way1: 共用全局变量
e.g.
```python
a = 0

def test1():
    # modify global variable
    global a
    a = 10

def test2():
    print(a)

test1()
test2()
```

Way2: 返回值作为参数传递
e.g.
```python
def test1():
    return 10

def test2(num):
    print(num)

result = test1()
test2(result)
```

#### **多个返回值**
e.g.
```python
def test1():
    return 1
    return 2
result = test1()
print(result)   # 1 只执行了第一个return，因为return退出当前函数，导致下方的代码不执行

def test2():
    return 1,2  # 正确的多个return值的写法，返回多个数据默认元组类型
result = test2()
print(result)
```

### **参数传递**
**位置参数**：调用函数时根据函数定义的参数位置来传递参数。
**关键字参数**：函数调用通过“键=值”的形式指定，可以让函数调用更清晰，同时不要求参数的顺序。
**缺省参数**：也称默认参数，用于定义函数时为参数提供默认值，调用函数时可以不传该默认参数的值。所有位置参数必须出现在默认参数前，包括函数定义和调用。函数调用时，如果为缺省参数被传值则修改默认参数值；否则使用这个默认值。
**不定长参数**：也称可变参数，用于不确定调用的时候会传递几个参数(不传参也可以)的场景。
可用包裹(packing)位置参数，或者包裹关键字参数，来进行参数传递，会显得非常方便。传入的所有参数都会被args变量按照位置打包为一个tuple。还可用包裹关键字传递。这都是**组包**的过程。


e.g. 
```python
# 位置参数
def welcome(name, mon, day):
    print(f'Hi {name}! Today is {mon}.{day}. Welcome!')

# 缺省参数
def welcome2(name, mon, day=28):
    print(f'Hi {name}! Today is {mon}.{day}. Welcome!')

# 包裹位置传递
def pakg(*args):
    print(args)

# 包裹关键字传递
def pakg2(**kwargs):
    print(kwargs)

# 位置参数
welcome('Jack', 'Jul', 28)
# 关键字参数
welcome(day=28, name='Jack', mon='Jul')
# 缺省参数
welcome2('Jack', 'Jul')  # Hi Jack! Today is Jul.28. Welcome!
welcome2('Jack', 'Jul', 30)  # Hi Jack! Today is Jul.30. Welcome!
# 包裹位置传递
pakg('Jack', 'Jul', 28)  # ('Jack', 'Jul', 28)
# 包裹关键字传递
pakg2(name='Jack', day=28, mon='Jul')  # {'name': 'Jack', 'day': 28, 'mon': 'Jul'}
```

**拆包**
e.g. 字典拆包取出来的是key
```python
dict1 = {'name': 'Jack', 'day': 28}
a, b = dict1
print(a) # name
print(b) # day
print(dict1[a]) # Jack
print(dict1[b]) # 28
```

### **引用**
在python中值是靠引用来传递的。可以用id()来判断两个变量是否为同一个值的引用。 可以将id值理解为那块内存的地址标识。**不可变类型与可变类型的引用结果有本质的差别！**
e.g.
```python
## int引用
a = 1
b = a
print(b) # 1
print(id(a), id(b))  # 140720565232016 140720565232016 内存地址相同
a = 2
print(b) # 1,说明int类型为不可变类型
print(id(a), id(b))  # 140720565232048 140720565232016 内存地址不同
b = 3
print(a) # 2
print(id(a), id(b))  # 140720565232048 140720565232080 内存地址不同

## list引用
a = [10, 20]
b = a
print(id(a), id(b))  # 2217640153480 2217640153480 内存地址相同
a.append(30)
print(b)  # [10, 20, 30], 列表为可变类型
print(id(a), id(b))  # 2217640153480 2217640153480 内存地址相同

## 引用作为实参
def test1(a):
    print(a)
    print(id(a))
    a += a
    print(a)
    print(id(a))

# int：计算前后id值不同
b = 100
test1(b)
# list：计算前后id值相同
c = [11, 22]
test1(c)
```

**可变和不可变类型**
可变类型：list; dict; set
不可变类型：int; float; string; tuple

**交换变量值**
e.g. 交换a=1 and b=2 
```python
# Way1: 引入第三个变量
a, b = 1, 2
c = 0
c = a
a = b
b = c

print(a, b)  # 2 1

# Way2: 
a, b = 1, 2
a, b = b, a
print(a, b)  # 2 1
```

### **高级特性**



### **lambda function 匿名函数**
如果一个函数仅有一个返回值，并且一句代码即可写成，则可以使用lambda简化代码。另外lambuda相比def可以节约内存空间。
lambda表达式的参数可有可无。lambda函数能接收任何数量的参数但只能返回一个表达式的值。

    function_name = lambda parameters: expressions 

e.g. no parameter
```python
def func1():
    return 100
print(func1)  # <function func1 at 0x0000014C26D80B88>
print(func1())  # 100

func2 = lambda: 100  # no parameter list
print(func2)  # func2 is the name address of the lambda function: <function <lambda> at 0x0000014C26D9E8B8>
print(func2())  # call lambda function: 100 
```

e.g. add two numbers with input parameters
```python
# Way1:
def func1(a, b):
    return a+b
print(func1(1,2)) 

# Way2:
func2 = lambda a,b: a+b
print(func2(1,2))

# Way3:
print((lambda a,b: a+b)(1,2))
```

e.g. defualt parameters  默认/缺省参数
```python
print((lambda a,b,c=100: a+b+c)(10,20))  # 130
print((lambda a,b,c=200: a+b+c)(10,20))  # 230
```

e.g. variable parameters 可变参数
```python
func1 = lambda *args: args
print(func1('Jack',100))  # ('Jack', 100)

func2 = lambda **kwargs: kwargs
print(func2(name='Jack', score=100))  # {'name': 'Jack', 'score': 100}
```

e.g. if statment in a lambda function
```python
func1 = lambda a,b: a if a > b else b
print(func1(20,10))  # 20
```

e.g. sorting by key values
```python
stu = [
{'name': 'Tom', 'age': 20},
{'name': 'Derek', 'age': 19},
{'name': 'Jack', 'age': 22}]

func1 = lambda x: x['name']
print(func1(stu[1]))  # Rose
stu.sort(key=func1)
print(stu)  # [{'name': 'Derek', 'age': 19}, {'name': 'Jack', 'age': 22}, {'name': 'Tom', 'age': 20}]

stu.sort(key=func1, reverse=True)
print(stu)  # [{'name': 'Tom', 'age': 20}, {'name': 'Jack', 'age': 22}, {'name': 'Derek', 'age': 19}]
```

### **高阶函数**
把函数作为另外一个函数的形式参数传⼊，这样的函数称为高阶函数，高阶函数是函数式编程的体现。函数式编程就是这种高度抽象的编程范式。
函数式编程大量使用函数，减少了代码的重复，因此程序比较短，开发速度较快，函数灵活性高。

e.g. add numbers
```python
def sum_num(a, b, f):
    return f(a) + f(b)

print(sum_num(-1,2,abs))  # 先求绝对值再求和: 3
print(sum_num(2.9,3.1,round))  # 先四舍五入再求和: 6
```

**内置高阶函数**
`map(func, list)`，将传入的函数变量func作用到list变量的每个元素中，并将结果组成迭代器地址(Python3)返回。 

e.g. 计算list序列中各个元素的2次方
```python
list1 = [1, 2, 3, 4, 5]
def func1(x):
    return x**2

result = map(func1, list1)
print(result)  # 内存地址： <map object at 0x0000018192FF7D48>
print(list(result))  # [1, 4, 9, 16, 25]
```

`reduce(func(x,y)，lst)`，其中func必须输入两个参数。每次func计算的结果继续和序列的下一个元素做累积计算。
e.g. 计算list序列中各个数字的累加
```python
import functools
list1 = [1, 2, 3, 4, 5]
def func(a, b):
    return a + b
result = functools.reduce(func, list1)
print(result) # 15
```

`filter(func, lst)` 用于过滤序列中不符合条件的元素, 返回一个 filter对象。可用list()转换为列表。
e.g. 筛选序列中的偶数
```python
list1 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

def func1(x):
    return x%2 == 0

result = filter(func1, list1)
print(result)  # <filter object at 0x00000188D2847E88>
print(list(result))  # [2, 4, 6, 8, 10]
```

### **装饰器**

### **偏函数**