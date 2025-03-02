# Pytest测试框架
### 一、pytest基本规则
* **测试用例文件(.py文件)要以test_开头或_test结尾**
* **测试用例类必须以Test开头，且不能有init方法**
* **测试用例以test_开头，一个测试用例类下面，可以存在多个测试用例**
* **断言使用pytest原生assert**
### 二、pytest常用参数
1. `-s`参数：可以在终端打印调试信息
```
# test_pytest_m.py
class TestPytest():
       def test_bing(self):
            print("bing","http://cn.bing.com")
```
```
pytest  -s testcase/test_pytest_m.py  # 可以打印出print输出内容
```
2. `-m`参数：只运行被标记的参数
    1. 如何标记？测试用例打标记: `@pytest.mark.标记名`
    2. 如何执行打标记的测试用例？`pytest -m "标记名"  `
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
### 三、pytest断言  
* 使用"== 、 != 、< 、 > 、>= 、<=”
* 使用in和not in 测试包含与不包含  
* 使用true和false
```
class TestAssert:
    def test_assert(self):
        # ==,!=,<,>,<=,>=
        assert "dd" == "dd"
        assert "aa" != "bb"
        assert 0 < 1
        assert 2 > 1
        assert 3 <= 7 - 2
        assert 4 >= 1 + 2
        # 包含和不包含
        assert "测开" in "从测试到测开"
        assert "java" not in "从测试到测开"
        # true和false
        assert 1
        assert (9<10) is True
        assert not False
```
### pytest精髓一：fixture
fixture的作用：可以将一个通用函数和其返回值，作为测试用例入参使用
    1. 使用方法：
```
@pytest.fixture()
def demo(self):
    print("执行测试用例")
    return 1
``` 
    2. 作用范围  
    session级别：这个级别会结合conftest.py文件使用  
    module级别：模块里所有的测试用例执行前只执行一次module级别的fixture  
    class级别：每个类执行前都会执行一次class级别的fixture  
    function级别：默认的函数级别，每个测试用例执行前都会执行一次function级别的fixture
### pytest精髓二：conftest  
1. conftest是可以跨文件使用的，也就是当前这个testcases目录下，每一个测试用例文件，都可以使用conftest
2. conftest.py这个文件名是固定的，不能更改
3. 用例使用conftest有一个默认原则，离哪个conftest最近就使用哪个一个。如果某个用例的同级目录有conftest，就使用这个conftest。同级目录没有conftest时，就往上级目录找最近的一个conftest
4. conftest不能被其他文件导入
5. conftest可以设置很多pytest内置的一些钩子方法