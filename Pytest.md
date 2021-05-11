---
title: Pytest
created: '2021-05-10T05:45:39.690Z'
modified: '2021-05-11T06:51:26.717Z'
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

import pytest
 
 
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


#### log 日志记录

通过命令行参数 --log-file 记录日志

```ini
[pytest]
addopts = -s -v --html=report/report.html --self-contained-html --log-file=./logs/log.log
log_cli=true
log_level=NOTSET
```


如果我们事先知道测试函数会执行失败，但又不想直接跳过，而是希望显示的提示
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


