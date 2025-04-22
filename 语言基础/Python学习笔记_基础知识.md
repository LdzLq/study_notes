1. python序列的**简单赋值**和**切片**操作，都不是完全的拷贝原数组，和原数组仍有关系。
2. 

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