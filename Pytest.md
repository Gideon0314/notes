---
title: Pytest
created: '2021-05-10T05:45:39.690Z'
modified: '2021-05-11T03:19:23.306Z'
---

### Pytest

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
# fixture内置两个参数
# scope='function'
# scope='class'
# scope='module'
# scope='session'

@pytest.fixture(scope='session', autouse=True)
def login():
  pass

def test_case_01(login):
  pass
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


