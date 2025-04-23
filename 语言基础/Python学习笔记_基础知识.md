# python学习笔记
## 基础语法  
### 学习资料
1. [Python 教程](https://docs.python.org/zh-cn/3/tutorial/index.html#tutorial-index)
1. [Python 语言参考手册](https://docs.python.org/zh-cn/3/reference/index.html#reference-index)
1. [术语对照表](https://docs.python.org/zh-cn/3/glossary.html#glossary)
1. [Python 标准库](https://docs.python.org/zh-cn/3/library/index.html#library-index)
1. [扩展和嵌入 Python 解释器](https://docs.python.org/zh-cn/3/extending/index.html#extending-index)
1. [Python/C API 参考手册](https://docs.python.org/zh-cn/3/c-api/index.html#c-api-index)
1. [Python 3 标准库实例教程](https://learnku.com/docs/pymotw)
1. [Python 包索引](https://pypi.org/)

### 基础语法
1. python序列的**简单赋值**和**切片**操作，都不是完全的拷贝原数组，和原数组仍有关系。
2. python函数定义和调用：函数调用时`*args`表示解包一个元组或列表，作为**位置参数**；`**args`表示解包一个字典，做为**关键值参数**
3. python队列：`collections.deque`,`deque.append()`队尾添加元素，`deque.popleft()`队首去除元素
4. `zip(*iterables, strict=False)`函数:对多个迭代器并行迭代，并从每个迭代器返回一个数据组成元组；`zip()`函数的工作：把行变成列，把列变成行，类似于矩阵转置；
       * `strict=False`是匹配最短长度的迭代器；`strict=True`是迭代器等长时检查使用，不等长则会报错
	   * `itertools.zip_longest(*iterables, fillvalue=None)`函数：可以对长度较短的迭代对象，填充可用常量
	举例：```
	list(zip(range(3), ['fee', 'fi', 'fo', 'fum']))
	```
5. `del`语句：可以`del a[0]`，`del a[2:4]`，`del a[:]`，`del a`；分别是删除一个值，删除切片对应值，删除所有值，删除整个列表变量
6. 元组：一对空圆括号`()`可以创建空元组;只有一个元素的元组可以再元素后面再加一个逗号`('hello',)`表示一元素元组;`x,y,z = t`可以称为序列解包
7. 集合：空集合只能用`set()`，`{}`创建的是空字典；`set('abracadabra')`会自动去除重复元素
       * `a - b`: 存在于 a 中但不存在于 b 中的字母；差集
	   * `a | b`: 存在于 a 或 b 中或两者中皆有的字母；并集
	   * `a & b`: 同时存在于 a 和 b 中的字母；交集
	   * `a ^ b`: 存在于a或b但非两者同时皆有的字母：补集

### 迭代器
1.描述：
> 1. 可以使用 **for循环** 遍历输出的变量，都是 **可迭代类型Iterable**
> 1. 可以使用 **next()** 函数的对象，都是 **迭代器类型Iterator**，表示惰性序列
> 1. 一般如 `list、dict、str`等都是**Iterable**，但不是**Iterator**，可以通过函数`iter()` 获得**Iterator对象**  

2.代码  
```
iterator_value = iter([1,2,3,4])  # 转化迭代器
while True:
    try:
        x= next(iterator_value)
    except StopIteration:  # 迭代器没有元素再输出，报错
        break
```

### 装饰器
装饰器，decorator，作用在函数、类上，功能是给函数动态增加功能；例如：类函数装饰器`@staticmethod`  
种类：
* 无参数型
```
def log(func):
    @functools.wraps(func)  # 需要导入import functools
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper
```
其中func、wrapper一般都不需改变，@functools.wraps(func)表示不修改原函数的__name__值
* 有参数型
```
def log(text):
    def decorator(func):
        @functools.wraps(func)  # 需要导入import functools
        def wrapper(*args, **kw):
            print('%s %s():' %( text, func.__name__))
            return func(*args, **kw)
        return wrapper
    return decorator
```

### lambda表达式
#### 1.定义
lambda是一种简单的函数，可以输出多个参数，但只能有一个表达式，并且返回表达式的结果
```
lambda 任意数量参数: 表达式
```
#### 2.可以和lambda表达式结合使用的
1. map()函数：接收一个函数和一个或多个可迭代对象作为参数，并返回新的迭代器
```
list(map(lambda x: x ** 2, numbers))
```
2. filter()函数：接收一个布尔表达式和一个可迭代对象作为参数，并返回一个新的迭代器
```
list(filter(lambda x: x % 2 == 0, range(10)))
```
3. sorted()函数：可以对列表进行排序，也可以接收一个参数作为关键字来指定如何进行比较
```
sorted(numbers, key=lambda x: abs(x))
```
4. reduce()函数：接收一个二元操作和一个可迭代对象作为参数，并返回累积的结果(用于对序列进行累积操作)
```
from functools import reduce
product = reduce(lambda x, y: x * y, [1, 2, 3, 4])
print(product)  # 输出: 24
```