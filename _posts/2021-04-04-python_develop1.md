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
print(f'My name is {name} and will be {age + 1} next year')
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
# a = a + 1 = 101
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
**while**
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

**for**
    for 临时变量 in 序列：
        重复执行的代码

**Exit loop**
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

**循环和else的配合使用**
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
print(name[-4:-1]) # def
print(name[::-1]) # gfedcba
```

- Find string
    string.find(⼦串, 开始下标, 结束下标)

rfind()：与find()功能相同，但查找⽅方向为右侧开始
rindex()：与index()功能相同，但查找⽅方向为右侧开始
count()：返回某个⼦串在字符串中出现的次数

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
print(mystr)  # Welcome to this blog and have a good time and enjoy!
```
*从上例注意到，字符串本身并没有被修改，除非赋值给一个新的变量，这说明**字符串是不可变的数据类型**

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

## List
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
    string.pop(): 删除指定索引的数据，默认为最后一个，并返回该删除的数据
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

alist.remove('b')
print(alist)  # ['c']

alist.clear()
print(alist)  # []
```

- 修改
    string.reverse(): 逆置
    string.sort(key=None, reverse=False): 排序：reverse = True 降序， reverse = False 升序
















