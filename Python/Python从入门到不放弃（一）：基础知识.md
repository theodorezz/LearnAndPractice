# Python基础知识
- 时代在变革，企业发展定会从更节约人力物力、更高效、更高品质的角度前行，传统纯手工测试效率低、隐藏缺陷可能未被发现的风险，只能靠增加工作时长、重复劳动来填补，测试人员也许面临着被“优化”隐患。
- 测试开发工程师，可能名称上有不同，但内涵是一致的：熟悉业务逻辑、测试技能的同时，具备代码能力，开发测试框架/工具、自动化测试脚本的编写及执行。将重复冗杂的工作交给自动化，人工处理关键复杂场景测试，高效配合，也是行业趋势。
- Python语言近年大火，用途甚广：Web开发（YouTube）、深度学习（AI）、爬虫、数据分析、自动化脚本。作为脚本语言，Python自带的UnitTest库即可做单元测试，也可做接口自动化，基于Urllib开发的第三方Requests库更是测试最常用库之一，Selenium/Appium也是UI自动化的主流，再加上最关键的对新手友好，可以得出结论：学。
- 本文记录了学习极客时间尹会生老师Python课的一些关键知识点，之后会更新将Python用于接口自动化的实际应用场景。
- 我在Github同步了系列文章，欢迎交流。
https://github.com/theodorezz/LearnAndPractice
![Python基础](https://img-blog.csdnimg.cn/20200501141210910.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dpbmRzZWV5b3U=,size_16,color_FFFFFF,t_70)

## 1.变量

### 变量定义和操作

- 全局变量
- 局部变量：只在函数执行期间生效

## 2.数据类型

### 1）工厂函数

- type()
- int()
- float()
- str()
- list()
- dict()
- bool()
- set()

### 2）int

### 3）str

### 4）list

- 示例：[1,2,3]
- 常用方法

	- list.append()
	- list.len()
	- list.insert()
	- list.remove()
	- list.sort()
	- list.count()

- 列表推导式

```python
alist = [ ]
for i in range(1,11):
    if (i % 2 == 0):
        alist.append( i*i )
 
 以上代码等价于
 blist = [i*i for i in range(0,10) if(i%2)==0]
```


- 可变序列

### tuple

- 示例：(1,2,3)
- 不可变序列

### dict

- 常用方法

	- dict.items()
	- dict.keys()
	- dict.values()

- 字典推导式

	- dict2={.....}---dict1 = {i:0 for i in dict2}

## 3.条件与循环


## 4.文件内建函数

### open( )

### read( ) 
### readline( )

- 读取一行

### readlines( )

### seek( )

- 文件内移动(文件指针/类似光标)

### write( )

- 写入

### close( )

## 5.错误和异常

### 检测和处理

```python
#finally不论如何都执行以下命令
try:
    a = open(’name.txt’)
except Exception as e:
    print(e)
finally:
    a.close()
```

### 抛出异常

- raise()

## 6.函数

### 1）关键字参数： *arg

```python
可以传入0个或任意和参数，参数在被函数调用时自动组成一个tuple

有一组数字a,b,c...计算a*a+b*b+c*c...
def calc(numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum

传参时需先组装列表或元组：
>>>calc([1,2,3])
>>>calc((1,2,3))

若调用可变参数，则传参可简化为：
>>>calc(1,2,3)

于是函数优化为：
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
也可传入0个参数
>>>calc()
>>>0

若传入参数需为列表时：
>>>num = [1,2,3]
>>>calc(num[1],num[2],num[3])
或更优雅的方式
>>>calc(*num)
```

### 2）关键字参数：  **kwargs

```python
可以传入0个或任意和参数，参数在被函数调用时自动组成一个dict
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)

传参时可只传name和age，也可传多个参数
>>> person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}

与可变参数类似，传参时可直接调用
>>> extra = {'city': 'Beijing', 'job': 'Engineer'}
>>> person('Jack', 24, **extra)
name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer’}

**extra表示把extra这个dict的所有key-value用关键字参数传入到函数的**kw参数，kw将获得一个dict，注意kw获得的dict是extra的一份拷贝，对kw的改动不会影响到函数外的extra。
```

### 3）命名关键字参数

```python
函数内部可以传入不受限制的参数，具体传入什么参数通过kw检查

person( )函数，希望可以检查是否有city 和 job函数
def person(name, age, **kw):
    if 'city' in kw:
        # 有city参数
        pass
    if 'job' in kw:
        # 有job参数
        pass
    print('name:', name, 'age:', age, 'other:', kw)

调用者仍可传入不受限制的关键字参数
>>> person('Jack', 24, city='Beijing', addr='Chaoyang', zipcode=123456)

如果要限制关键字参数的名字，可以使用命名关键字参数，例如只接受city和job为关键字参数
def person(name, age, *, city, job):
    print(name, age, city, job)

*后面的参数视为命名关键字参数
>>> person('Jack', 24, city='Beijing', job='Engineer')
Jack 24 Beijing Engineer

命名关键字必须传入参数名，否则报错
>>> person('Jack', 24, 'Beijing', 'Engineer')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: person() takes 2 positional arguments but 4 were given

命名的参数可以有默认值，简化调用
def person(name, age, *, city='Beijing', job):
print(name, age, city, job)
```

### 4）生成器
```python
def frange(start, stop, step):
    x = start
    while x < stop:
        yield x   #yield生成器也是一种迭代器
        x =+ step

for i in frange(10,20,0.5):
    print(i)

#当10小于20时打印0.5，每次+0.5直到start=stop时停止
```

### 5）匿名函数/lambda表达式

```python
def add(x,y):
    return x+y
add(3,5)

#以上等价于
lambda x,y : x+y(3,5)
```

### 6）迭代器
- next()
- iter()
- list()

```python
list1 = [1,2,3]
it = iter(list1)
print(next(it))
print(next(it))
print(next(it))
…
直至报错
```

### 7）内建函数

- filter( )   取出满足func的元素

```python
a = [1,2,3,4,5,6,7]

print(list(filter(lambda x:x>2,a)))
[3,4,5,6,7]
```

- map( ) 对参数依次处理

```python
print(list(map(lambda x:x+1,a))
[2,3,4,5,6,7,8]
--------------------
a=[1,2,3]
b=[4,5,6]
print(list(map(lambda x,y:x+y, a,b)))
[5,7,9]
```

- reduce( ) 对参数序列中元素进行累积

```python
from functools import reduce
reduce(lambda x,y:x+y, [2,3,4],1)
10
-------
等价于
（（1+2）+ 3 ）+ 4
```

- zip( ) 矩阵转换

```python
for i in zip((1,2,3),(4,5,6))
    print(i)
(1,4)
(2,5)
(3,6)
————
矩阵转换
(1,2,3)
(4,5,6)

字典key-value值调换
dicta = {‘a’:”aa”, ‘b’=“bb”}
dictb = zip(dicta.values(), dicta.keys())
print(dict(dictb)
{‘aa’:’a’, ‘bb’:’b'}
```

### 8）闭包
内部函数引用外部变量

```python
def counter():
    cnt = [0]
    def add_one():
        cnt[0] += 1
        return cnt[0]
    return add_one
num1 = counter()
print(num1())
```

### 9）装饰器

- 基于闭包，先执行闭包函数再执行本函数

```python
import time
def timer(func):
    def wrapper():
        start_time = time.time()
        func()
        stop_time = time.time()
        print(“运行时间上 %s 秒” %(stop_time - start_time))
    return wrapper

@timer
def i_can_sleep():

i_can_sleep()
```

### 10）上下文管理器

```python
fd=open(’name.txt’)
try:
    for line in fd:
        print(line)
finally:
    fd.close()

with open(’name.txt’) as f:
    for line in f:
        print(line)
（如果打开失败会自动调用finally）
```

## 7.模块导入



## 8.面向对象编程

### 1）类

- 类可直接调用类中定义的函数： `class Player( ):`


- __init__表示调用类时的预操作：`def __init__(self, name,age):`

- 类的实例化：`user = Player('theodore',24)`

### 2）类对象

- self：表示实例化后实例的本身，是一个固定值

### 3）继承：
- class 类a(类A)：

## 9.多线程

### 常用库

- threading
- queue

### 常用方法

- start()
- join()

## 10.标准库

### 1）运行服务的__main__

### 2）文字处理（正则）re库

- 元字符

	

```python
*    匹配字符出现0次到多次：ca*t
.    匹配单个字符：c.t
^    以…开头：^cat
$    以…结尾：cat$
+    出现1次到多次
?    出现0次到1次:c?t
{ }  表示字符出现指定次数：ca{4}t
{,}  匹配范围次数ca{4,6}t
[ ]  匹配固定字符：c[bcd]t   cbt，cct，cdt
|    字符选择左边还是右边
\d   单个数字
\D   不包含数字
\s   匹配空白
( )  提取( )中内容：(03|04)
^$   表示该行为空行
- .*?   不使用贪婪模式：如只匹配较短字符串ca*?t匹配caaaaaat只能匹配到cat
```
```python
import re

p = re.compile(‘ca*t’)
print (p.match(‘caaaaat’))
#可以正常匹配

p = re.complie(r’(\d+)-(\d+)-(\d+)’)  #r防止转义
print (p.match(‘2018-05-10’))  #匹配整个字符串
print ((p.match(‘2018-05-10’)).group(1)) #取出第1个括号包扩的内容
print ((p.match(‘2018-05-10’)).groups()) #取出所有字符串输出为元组：(‘2018’,’05’,’10')
```
- 常用函数

	- group()/match()
	- search()
	- findall()
	- 字符替换`sub('abc','!!!')`

```python
print (p.search(‘aa2018-05-10bb’))
#可与p匹配

phone = ‘123-456-789 #这是电话号码'
p2 = re.sub(r’#.*$’,’’,phone)  #替换注释
```

### 3）日期类型time/datetime

- time

	- time()
	- localtime()

- datetime

	- datetime()
	- timedata()
	- now()

### 4）数字和数学类型math/random

- random

	- randint()
	- choice()
```python
import random
random.randint(1,5)
random.choice([‘aa’,’bb’,’cc'])
```

### 5）文件和目录访问pathlib/os.path

- os.path

	- abspath()
	- exists()
	- isfile()
	- isdir()
	- join()
```python
#os.path
import os
os.path.abspath(‘..’)   #..对应的绝对路径
os.path.isfile(‘/Users/file’)   #是不是文件
os.path.join(‘/tmp/a’,’b/c’)  #连接路径

#pathlib
from pathlib import path
p = path(‘.’)
p.resolve()   #点对应的绝对路径

#新建目录
q = Path(‘/tmp/a/b/c’)
Path.mkdir(q, parents=True)  #parents表示父目录是否存在，不存在自动创建
```

### 6）数据压缩及归档tarfile

### 7）unittest
### 8）name，main

```python
#直接python执行，则会被执行，作为模块导入则不会执行
if  __name__ ==  ‘__main__’:
    func1()
    func2()
```

## 11.第三方库
### 1）urllib

```python
from urllib import request

url = ‘http://www.baidu.com'
response = requests.urlopen(url,timeout=1)
print(response.read().decode(‘utf-8’))
```

### 2）requests

```python
#get请求
import requests
url = ‘http://httpbin.org/get'
data = {‘key’:’value’,’abc’:’xyz’}
#  .get是使用get方式请求url，字典类型的data不用进行额外处理
response = requests.get(url,data)
print(response.text)


#post请求
import requests
url = ‘http://httpbin.org/get'
data = {‘key’:’value’,’abc’:’xyz’}
#  .post表示为post方法
response = requests.post(url,data)
# 返回类型为json格式
print(response.json())
```

### 3）pytest


