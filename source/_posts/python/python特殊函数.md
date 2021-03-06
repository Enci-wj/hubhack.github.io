---
title: python特殊函数


categories: 
- python
tags:
- python
---

# 匿名函数
匿名函数 :隐藏名字,即没有名称
匿名函数: 没有名字的函数
lambda表达式
python中,使用lambda表达式构建匿名函数
使用 lambda关键字定义匿名函数,格式为lambda[参数列表]:表达式
参数列表不需要小括号,无参就不写参数
冒号用来分割参数和表达式部分
不需要使用return,表达式的值,就是匿名函数的返回值.表达式中不能出现等号.
lambda表达式匿名函数只能写在一行上,也称为单行函数.

匿名函数往往用在高阶函数传参时,使用lambda表达式,往往能简化代码.

# 递归函数

函数执行流程:压栈

递归函数recursion
函数直接或者间接调用自身就是递归.
递归需要有边界条件,递归前进段,递归返回段.
递归一定要有边界条件.
当边界条件不满足的时候,递归前进
当边界条件满足的时候,递归返回.

递归例子:斐波那契数列 
如果用循环写法:
```
a = 0
b = 1
n = 10
for i in range(n -1):
    a,b=b, a+b
else:
    print(b)

```

递归写法:
def fib(n):
    return 1 if n <3 else fib(n-1) +fib(n-2)

递归要求
递归一定要有退出条件,递归调用一定要执行到这个条件,没有退出条件的递归调用.
递归调用的深度不宜
python 对递归调用的深度做了限制,以保护解释器
超过递归深度限制,抛出recursionError:maxinum recursion depth exceeded 超过最大深度
sys.getrecursionlimit()

循环稍微复杂一些,但是只要不是死循环, 可以多次迭代直至算出结果.
递归还有深度限制, 如果递归复杂, 函数反复压栈 ,栈内存很快就溢出了.

递归是一种很自然的表达,符合逻辑思维
递归相对效率低,每一次调用都要开辟栈帧.
递归有深度限制,如果递归层次太深,函数反复压栈,栈内存很快就溢出了.
如果是有限次数的递归, 可以使用递归调用, 或者使用循环代替 ,循环代码稍微复杂一些,但是只要不是死循环,可以多次迭代直至算出结果.
绝大多数递归,都可以使用循环实现
即使递归代码很简洁,但是能不用则不用递归.

# 生成器

生成器generator
生成器指的是生成器对象,可以由生成器表达式得到,也可以使用yield关键字得到一个生成器函数,调用这个函数得到一个生成器.
生成器对象,是一个可迭代对象,是有个迭代器.
生成器对象,是延时计算,惰性求值的.

函数体中包含yield语句的函数,就是生成器函数,调用后返回生成器对象.
普通函数调用,函数会立即执行直到执行完毕.
生成器函数的调用,并不会立即执行函数体, 而是需要使用next函数来驱动生成器函数来执行获得的生成器对象.
生成器表达式和生成器函数都可以得到生成器对象,只不过生成器函数可以写的更加复杂的逻辑.

在生成器函数中,可以多次yield,没执行一次后会暂停执行,把yield表达式的值返回
再执行会执行到下一个yield语句又会暂停执行
return会导致当前函数返回,无法继续执行,也无法继续获取下一个值,抛出stoplteraton异常.
如果函数没有显示的return语句如果生成器函数执行到结尾,一样会抛出stoplterration
生成器函数
包含yield语句的生成器函数调用后,生成器对象的时候,生成器函数的函数体不会立即执行
next会从函数的当前位置向后执行到之后碰到的第一个yield语句,会弹出值,并暂停函数执行.
再次调用next函数,和上一条一样的处理过程.
继续调用next函数,生产器函数如果结束执行了,会抛出stoplteration异常.

协程
生成器的高级用法
他比进程,线程轻量级,是在用户空间调度函数的一种实现.
协程调度器实现思路
协程是一种非抢占式调度.

yield from 语法
yield from就是一种简化语法的语法糖.

# yield 和return的区别

带有yield 的函数在python被称为generator,return和yield的形式和用法很像,下面fib数列来对比一下yield 和return

如何生成斐波那契数列:
斐波那契（Fibonacci）数列是一个非常简单的递归数列，除第一个和第二个数外，任意一个数都可由前两个数相加得到。用计算机程序输出斐波那契數列的前 N 个数是一个非常简单的问题，许多初学者都可以轻易写出如下函数.

```
def fab(max):
    a, b = 1, 1
    n = 1
    while n < max:
        print(b)
        a, b = b, a+b
        n +=1
fab(20)
```
结果没有问题, 但有经验的开发者指出,直接在fab函数中用print打印数字会导致该函数可复用性较差, 因为fab函数返回none, 其他函数无法获得该函数生成的数列.
要提高fab函数的可复用性, 最好不要直接打印出数列,而是返回一个list.以下fab函数改写后的第二个版本.
```
def fab(max):
    n, a, b = 0, 0, 1
    l = []
    while  n < max:
        l.append(b)
        b = b+a
        a = b-a
        n = n+1
    return l
```
改写后的fab函数通过返回list能满足复用性的要求, 但是更有经验的开发者会指出,
该函数在运行中占用的内存会随着参数max的增大而增大, 如果要控制内存占用,最好不要
用list来保存中间结果, 而是通过iterable对象来迭代.

```
class Fab(object): 
 
   def __init__(self, max): 
       self.max = max 
       self.n, self.a, self.b = 0, 0, 1 
 
   def __iter__(self): 
       return self 
 
   def next(self): 
       if self.n < self.max: 
           r = self.b 
           self.b = self.a + self.b
           self.a = self.b - self.a
           self.n = self.n + 1 
           return r 
       raise StopIteration()

```
然而,使用class改写的这个版本,代码远远没有第一版的fab函数来的简洁,如果我们想要保持第一版fab函数的简洁性, 同时又要获得iterable的效果, yield就派上用场了:
def fab(max):
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        b =  b+a 
        a = b-a
        n = n + 1

第四个版本的fab和第一版相比, 仅仅把print b改为 yield b, 就在保持简洁性的同时获得了iterable的效果
调用第四版的fab和第二版的fab完全一致:

简单地将,yield的作用就是把一个函数变为一个generator, 带有yield的函数不再是一个普通函数, python解释器会将其视为一个generator, 调用fab不会执行fab函数,而是返回一个iterable对象 在for循环执行时 ,每次循环都会执行fab函数内部的代码,执行到yield b时,fab函数就返回一个迭代值,下次迭代时,代码yield的下一条语句继续执行,
而函数的本地变量看起来和上次中断执行前是完全一样的.于是函数继续执行,直到再次遇到yield.也可以手动调用fab(5)的next()方法
这样我们就可以清楚看到fab的执行流程.

# 高阶函数
函数也是对象, 是可调用对象
函数可以作为普通变量,也可以作为函数的参数, 返回值.

在数学和计算机科学中, 高阶函数应当是至少满足下面一个条件的函数
接受一个或多个函数作为参数
输出一个函数

排序sorted
排序函数,可以接受key作为参数进行排序,不改变序列的原有值.
排序也是在程序中经常用到的算法,key指定的函数将作用于list的每一个元素上,并根据key函数返回的结果进行排序,对比原始的list和经过key=abs处理过的list.
然后sorted()函数按照keys进行排序,并按照对应关系返回list相应的元素.

过滤filter
对可迭代对象进行遍历,返回一个迭代器
function参数是一个参数的函数, 且返回值应当是bool类型,或其返回值等效布尔值
function参数如果是None, 可迭代对象的每一个元素自身等效布尔值.

映射map
对多个可迭代对象的元素,按照指定的函数进行映射
返回一个迭代器


# 柯里化
指的是将原来接受两个参数的函数变为新的接受一个参数的函数的过程.新的函数返回一个以原有第二个参数为参数的函数.



