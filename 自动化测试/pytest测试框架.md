## Pytest学习总结
### 一、pytest测试用例编写规范：
* 测试用例文件是`test_*.py` 或者 `*_test.py`格式
* 测试函数/方法，以`test_`开头，不放在类中也可以；
* 放在类中，测试用例类以`Test`开头，且不能有`__init__`方法
* 自带断言`assert`，无需导入任何断言库
    * 使用`== 、 != 、< 、 > 、>= 、<=`
    * 使用`in`和`not in`测试包含与不包含  
    * 使用`true`和`false`

### 二、pytest命令行参数
实例代码：
```
# test_pytest_m.py

class TestPytest():

    @pytest.mark.bing
    def test_bing(self):
        print("bing","http://cn.bing.com")
    
    @pytest.mark.bing
    def test_baidu(self):
        print("baidu","http://www.baidu.com")

```

1. `-s`参数：可以在终端打印输出内容
```
pytest -s testcase/test_pytest_m.py  # 可以打印出print输出内容
```
2. `-m`参数：只运行被标记的参数
    1. 如何标记？测试用例打标记: `@pytest.mark.标记名`
    2. 如何执行打标记的测试用例？`pytest -m "标记名"  `
```
pytest -m bing
```

3. `-k`参数：模糊匹配文件名、类名、方法名，执行匹配到的方法  
```
 pytest -k 模糊匹配值
```
4. `-v`参数 ：打印更加详细的执行信息  
```
pytest -s -v -k 模糊匹配值
```
5. `--collect-only`参数：只罗列当前目录下的所有测试模块、测试类、测试函数  
```
pytest testcases --collect-only
```
6. `-q`参数：打印简洁的执行信息
```
pytest -q testcases/test_pytest.py
```

### pytest之fixture函数
1. 作用：fixture是pytest内置的一个装饰器，`@pytest.fixture`被用来定义一个测试用例执行前要做的准备工作
2. 特点：
    * fixture函数有scope范围：`function`, `class`,`module`, `package`, `session`，默认是function级别
        * function级别：每个测试用例执行前都会执行一次function级别的fixture  
        * class级别：每个测试类执行前都只会执行一次class级别的fixture  
        * module级别：每个模块执行前都只会执行一次module级别的fixture  
        * package级别：整个包执行前都只会执行一次package级别的fixture  
        * session级别：整个测试会话执行前都只会执行一次session级别的fixture  
    * fixture函数可以有返回值，测试用例可以直接使用返回值
    * fixture函数可以有传入参数，测试用例可以直接传参
    * fixture函数可以有多个，测试用例可以传入多个fixture参数
    * fixture函数可以给测试用例提供参数，也可以给其他fixture函数提供参数
    * fixture函数可以单独放在一个文件`conftest.py`中，也可以放在多个文件中
    * 同名的fixture函数，在当前目录下，后面的fixture会覆盖前面的fixture
3. 实现Teardown/Cleanup功能：
    * 使用`yield`关键字，在fixture函数中使用`yield`可以实现teardown/cleanup功能
        * 使用yield的fixture函数，销毁顺序是后进先出（类似于数据类型栈Stack），意思是：**最后执行的fixture，它的yield内容先运行**
    * 使用`request.addfinalizer()`方法，在fixture函数中使用`request.addfinalizer(lambda: 清理代码)`可以实现teardown/cleanup功能
        * 使用`request.addfinalizer()`的fixture函数，销毁顺序是先进先出（类似于数据类型队列Queue），意思是：**最先执行的fixture，它的清理代码最后运行**
4. fixture函数传入参数`request`，作用是可以获取与测试会话和参数化相关的信息，不可以用在用例函数中


### pytest之conftest文件
1. conftest是可以跨文件使用的，也就是当前这个testcases目录下，每一个测试用例文件，都可以使用conftest
2. conftest.py这个文件名是固定的，不能更改
3. 用例使用conftest有一个默认原则，离哪个conftest最近就使用哪个一个。如果某个用例的同级目录有conftest，就使用这个conftest。同级目录没有conftest时，就往上级目录找最近的一个conftest
4. conftest不能被其他文件导入，意思是测试用例会自动识别并加载conftest文件，不用特别导入
5. conftest可以设置很多pytest内置的一些钩子方法

### pytest之参数化
用例参数化的两种方法：  
1. `pytest.fixture()`可以设置参数化，
`@pytest.fixture(params=[1,2,3], ids=['a','b','c'])，params是参数列表，ids是参数的别名`

2. 使用`@pytest.mark.parametrize()`装饰器
    * 参数化单个函数：
    ```
    @pytest.mark.parametrize("test_input,expected", [("3+5", 8), ("2+4", 6), ("6*9", 42)])
    def test_eval(test_input, expected):
        assert eval(test_input) == expected
    ```
    * 参数化单个类：
    ```
    @pytest.mark.parametrize("n,expected", [(1, 2), (3, 4)])
    class TestClass:
        def test_simple_case(self, n, expected):
            assert n + 1 == expected

        def test_weird_simple_case(self, n, expected):
            assert (n * 1) + 1 == expected
    ```
    * 参数化多个函数：
    ```
    pytestmark = pytest.mark.parametrize("n,expected", [(1, 2), (3, 4)])

    class TestClass:
        def test_simple_case(self, n, expected):
            assert n + 1 == expected

        def test_weird_simple_case(self, n, expected):
            assert (n * 1) + 1 == expected
    ```