---
title: Pytest
created: '2021-05-10T05:45:39.690Z'
modified: '2021-05-11T07:40:15.588Z'
---

## Pytest

#### 常用命令行参数

```
pytest -v (打印详细的日志信息)

pytest -s（将用例中print方法打印的信息输出到控制台）

pytest test_case 执行test_case文件夹中的所有测试用例

pytest test_case/test_01.py::TestCase::test_step_01 单独执行类中方法

pytest -k "类名" and not 方法名"跳过某个方法执行测试用例

pytest -m [标记名]  执行被@pytest.mark.[标记名]装饰的测试用例
pytest.ini 里也需要加参注册标记
markers = p1: mark a test

pytest -x 文件名 用例执行失败，就停止执行剩余用例

pytest --maxfail=[num] 当错误用例数达到num时，停止执行用例

pytest -lf 运行上一次失败的用例

pytest -ff 先运行上一次失败的用例，再运行剩余的

pytest --cache-clear 清楚缓存

pytest -s --durations=0 -vv 查看每条用例的运行时间
```
#### pytest.fixture()的使用
```python
# fixture 内置两个参数
# scope='function'  函数级别，每一个函数调用一次
# scope='class'     类级别，每一个类调用一次
# scope='module'    模块级别，每一个py文件调用一次
# scope='session'   会话级别，在所以测试用例执行前调用一次
# autouse 默认为false，当为True时，无需调用即自动执行

@pytest.fixture(scope='session', autouse=True)
def login():
  token = ''
  return token

# 调用fixture,方法1
def test_case_01(login):
  pass

# 调用fixture，方法2
@pytest.mark.usefixtures(login)
def test_case_01(login):
  pass
```
#### pytest-repeat
```python
通过在函数上添加@pytest.mark.repeat(count)装饰，指定函数执行次数。

@pytest.mark.repeat(5)
def test_01(start, open_baidu):
    print("测试用例test_01")
```
#### pytest-rerunfailures
```python
# 在方法中添加该装饰器，使用功能：用例失败时，重新运行5遍,用例执行时间间隔两秒。
# 如果方法中添加了该装饰器，命令行执行时，重新运行次数及间隔时间与装饰器配置不符，以装饰器的为准
@pytest.mark.parametrize("a,b", [(1, 0)])
@pytest.mark.flaky(reruns=5, reruns_delay=2)
def test_div(a, b):
    print(a / b)
```
#### pytest-assume
```python
# pytest自带的校验工具assert，如果用例中包含多个断言
# 第一个断言失败时，其他断言则不会在执行
# 在实际工作中，我们希望用例的每个断言都得到执行,此时就需要多重校验插件pytest-assume。
# 示例：第二个断言失败，第三个仍然执行，代码如下：
# 第二条断言失败时，第三条依旧会校验

def test_assume():
    pytest.assume(1 == 1)
    pytest.assume(False)
    pytest.assume(True)
```
#### pytest-ordering
```python

# 在实际的工作过程中，我们会遇到需要按照一定顺序执行的用例
# 此时可以使用pytest-ordering来实现。如同一类中，有的用例指定了顺序
# 有的没有执行顺序，则先运行指定顺序的用例，之后再根据默认顺序执行剩余用例

class TestOrder:
    # 在该脚本中，test_a第三个执行，因为其余三条用例都指定了执行顺序
    def test_a(self):
        print("a")
 
    # 该用例第二个执行
    @pytest.mark.run(order=2)
    def test_002(self):
        print("002")
 
    # 该用例第一个执行
    @pytest.mark.run(order=1)
    def test_001(self):
        print("001")
 
    # 该用例最后执行
    @pytest.mark.run(order=-1)
    def test_fianal(self):
        print("fianal")
```

#### pytest-dependency
```python
# 在实际工作共经常会遇到用例执行有依赖关系，比如订单系统，查看订单前必须要创建订单。
# 示例：依赖关系为先创建订单，再查看订单，然后删除订单。具体代码如下：

class TestDepend:
    """
    业务场景为新增/查看列表/删除
    查看列表用例依赖新增用例
    删除用例依赖查看列表用例
    """
 
    # 定义用例的别名
    @pytest.mark.dependency(name="create")
    def test_create(self):
        print("create")
 
    # 定义用例的别名为list，需要依赖的用例create，
    # 如果create执行失败，则list用例不会再执行
    # 如果list执行失败，delete则不会再执行
    @pytest.mark.dependency(name="list", depends=["create"])
    def test_list(self):
        print("list")
        assert False
 
    # 定义用例的别名为delete，需要依赖的用例为"create", "list"
    @pytest.mark.dependency(name="delete", depends=["create", "list"])
    def test_delete(self):
        print("delete")
```

#### pytest-xdist 多进程，分布式
```python
# 分布式执行用例的前提：所有的用例都可以单独执行，用例间无依赖关系。当我们有大量的用例需要执行时，就可以# 采用分布式执行用例，来降低执行用例的总时长。
# 命令行执行使用命令:pytest [目录或文件名] -n [线程数]
# 示例：使用5个线程执行test_xdist.py文件。在命令行输入命令 pytest test_xdist.py -n 5
class TestXdist:
    @pytest.mark.parametrize("a", [1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
    def test_001(self, a):
        sleep(1)
        print(a)
```
#### pytest-parallel 多进程，多线程
```python
def test_01(start,open_web1):
  print('测试用例3操作')
def test_02(start,open_web1):
  print('测试用例4操作')

if __name__ == "__main__":
  pytest.main(["-s", "test_1.py",'--workers=1', '--tests-per-worker=3'])

# 参数值配置：
# --workers=n 多进程运行需要加此参数，n是进程数。默认为1 【注：在windows系统中只能为1】
# --tests-per-worker=n 多线程运行需要加此参数，n是线程数。
# 如果两个参数都配置了，就是进程并行，每个进程最多n个线程，总线程数：进程数*线程数
# pytest-parallel的workers参数在windows系统下永远是1，在linux和mac下可以取不同值。
```

### pytest.ini 配置


#### log 日志记录

通过命令行参数 --log-file 记录日志

```ini
[pytest]
addopts = -s -v --html=report/report.html --self-contained-html --log-file=./logs/log.log
log_cli=true
log_level=NOTSET
```


如果我们事先知道测试函数会执行失败，但又不想直接跳过，而是希望显示提示
```python
@pytest.mark.xfail
```
```python
xfail(condiition, reason, [raises=None, run=True, strict=False])
```

```python
import pytest


class TestCase(object):

    @pytest.mark.xfail(1 < 2, reason='预期失败， 执行失败')
    def test_case_01(self):
        """ 预期失败， 执行也是失败的 """
        print('预期失败， 执行失败')
        assert 0

    @pytest.mark.xfail(1 < 2, reason='预期失败， 执行成功')
    def test_case_02(self):
        """ 预期失败， 但实际执行结果却成功了 """
        print('预期失败， 执行成功')
        assert 1
```


