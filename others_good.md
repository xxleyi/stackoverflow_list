- [How to know if an object has an attribute in Python - Stack Overflow](https://stackoverflow.com/questions/610883/how-to-know-if-an-object-has-an-attribute-in-python/610923#610923)
长见识了，在很多时候，[EAFP `try/except`](https://web.archive.org/web/20180411011411/http://python.net/~goodger/projects/pycon/2007/idiomatic/handout.html#id49)
 is preferred...
```python
try:
    return str(x)
except TypeError:
    ...
```

---
- [python - methods of metaclasses on class instances - Stack Overflow](https://stackoverflow.com/questions/2242715/methods-of-metaclasses-on-class-instances)

![i](https://i.stack.imgur.com/KhYPc.png)

Schematically, it boils down to:
```
type -- object
  |       |
Meta --   A  -- a
```

---
[How to extract list between 2 list python 3? - Stack Overflow](https://stackoverflow.com/questions/56579862/how-to-extract-list-between-2-list-python-3)

挺精彩，三个答案三种方式：
* f string, zip
* map, lambda, \*agrs
* comprehesion, join, zip

---
[sqlalchemy - How to elegantly check the existence of an object/instance/variable and simultaneously assign it to variable if it exists in python? - Stack Overflow](https://stackoverflow.com/questions/6587879/how-to-elegantly-check-the-existence-of-an-object-instance-variable-and-simultan)

sqlalchemy way of exists:
```python
from sqlalchemy import exists
(ret, ), = Session.query(exists().where(SomeObject.field==value))
ret = Session.query(exists().where(SomeObject.field==value)).scalar()
```

---
[python - How can I multiply elements in one list while providing a range in another - Stack Overflow](https://stackoverflow.com/questions/56662199/how-can-i-multiply-elements-in-one-list-while-providing-a-range-in-another/56662688#56662688)

大道至简，我费劲心思优化之后取得结果反倒比这样直接暴力写出来的还要慢，看来 Python 背地里对 list += 做了优化
```python
l_name = ['Doc_1', 'Doc_2', 'Doc_3']
l_depth = [1, 3, 2]

l_doc = []
l_page = []

for i, j in zip(l_name, l_depth):
    l_doc.extend([i] * j)
    l_page.extend(range(1, j + 1))
```

---
[python - list comprehension in exec with empty locals: NameError - Stack Overflow](https://stackoverflow.com/questions/45132645/list-comprehension-in-exec-with-empty-locals-nameerror)

list comprehension create new local scope and exec in Python3 becomes real function. Very good question.

but [this answer](https://stackoverflow.com/a/45196415/7025361) is better than the accepted.

---
[JavaScript - Is it okay to define a property of a previously declared object during a variable declaration? - Stack Overflow](https://stackoverflow.com/questions/56615111/javascript-is-it-okay-to-define-a-property-of-a-previously-declared-object-dur/56615348#56615348)

JS 变量声明中的微妙之处。

---
[Representing graphs (data structure) in Python - Stack Overflow](https://stackoverflow.com/questions/19472530/representing-graphs-data-structure-in-python)

Represent Graph in Python

---
[python - How can I iterate over all possible values of a dictionary? - Stack Overflow](https://stackoverflow.com/questions/56860697/how-can-i-iterate-over-all-possible-values-of-a-dictionary/56861874#56861874)

使用递归简化代码复杂度的绝佳实例

```python
from itertools import product

def traverse(d):
    K,V = zip(*d.items())
    for v in product(*(v if isinstance(v,list) else traverse(v) for v in V)):
        yield dict(zip(K,v)) >>> d = {
>>>     "key0": {
>>>         "key1": [1, 2],
>>>         "key2": [8, 9, 10]
>>>     },
>>>     "key3": [22, 23, 24]
>>> }

>>> from pprint import pprint
>>> pprint([*traverse(d)])
```

---
2019-07-21 | Sunday | No.202 | Week.28

[data modeling - Choosing a partition key for a Cassandra table -- how many is too many partitions? - Stack Overflow](https://stackoverflow.com/questions/30648479/choosing-a-partition-key-for-a-cassandra-table-how-many-is-too-many-partition)

解惑了，partition 本来就是碎片化的，碎片化程度与选择的 partition key 无关，与节点个数倒是有一点关系。

---
2019-07-22 | Monday | No.203 | Week.29

[multithreading - How to combine python asyncio with threads? - Stack Overflow](https://stackoverflow.com/questions/28492103/how-to-combine-python-asyncio-with-threads)

其中第二个未被接受的答案的思路超级棒，稍加改造之后，便可以很舒服的用于将很多「不好改为异步或者纯cpu型」的同步函数封装为异步函数，提升代码整体性能。

```python
import time
import asyncio


class Executor:
    """In most cases, you can just use the 'execute' instance as a
    function, i.e. y = await execute(f, a, b, k=c) => run f(a, b, k=c) in
    the executor, assign result to y. The defaults can be changed, though,
    with your own instantiation of Executor, i.e. execute =
    Executor(nthreads=4)"""

    def __init__(self, loop=None, nthreads=1):
        from concurrent.futures import ThreadPoolExecutor

        if not loop:
            loop = asyncio.get_running_loop()
        self._ex = ThreadPoolExecutor(nthreads)
        self._loop = loop

    def __call__(self, f, *args, **kw):
        from functools import partial

        return self._loop.run_in_executor(self._ex, partial(f, *args, **kw))


def cpu_bound_operation(t, alpha=30):
    import time

    time.sleep(t)
    return 20 * alpha


async def async_wrapper_of_sync():
    # 3 threads for three tasks, which can be adjusted.
    execute = Executor(nthreads=3)
    x_task = asyncio.ensure_future(execute(cpu_bound_operation, 2, alpha=-2))
    y_task = asyncio.ensure_future(execute(cpu_bound_operation, 2, alpha=-2))
    z_task = asyncio.ensure_future(execute(cpu_bound_operation, 2, alpha=-2))
    x = await x_task
    y = await y_task
    z = await z_task
    # or use asyncio.gather
    # x, y, z = await asyncio.gather(*(execute(cpu_bound_operation, 2, alpha=-2) for i in range(3)))
    return x + y + z


async def main():
    res = await asyncio.gather(*(async_wrapper_of_sync() for i in range(4)))
    return res


start = time.time()
res = asyncio.run(main())
print(time.time() - start)
print(res)
```

---
[What is this kind of assignment in Python called? a = b = True - Stack Overflow](https://stackoverflow.com/questions/11498441/what-is-this-kind-of-assignment-in-python-called-a-b-true)

Python is different:

```
python
a = b = c
```
means
```
python
temp = c
a = temp
b = temp
```

---
[garbage collection - Python del statement - Stack Overflow](https://stackoverflow.com/questions/14969739/python-del-statement)

> python gc is only needed for reclaiming cyclic structures.

---
[django - Docker/Kubernetes + Gunicorn/Celery - Multiple Workers vs Replicas? - Stack Overflow](https://stackoverflow.com/questions/51610189/docker-kubernetes-gunicorn-celery-multiple-workers-vs-replicas)

k8s gunicorn celery setting best practice

---
[python - Concatenation of the result of a function with a mutable default argument - Stack Overflow](https://stackoverflow.com/questions/57593294/concatenation-of-the-result-of-a-function-with-a-mutable-default-argument)

help me understand evaluation in python deeper

---
[How to print colored text in terminal in Python? - Stack Overflow](https://stackoverflow.com/questions/287871/how-to-print-colored-text-in-terminal-in-python/50025330#50025330)

very useful, **Simple usage: print(fg("text", 160))**

```python
# Pure Python 3.x demo, 256 colors
# Works with bash under Linux and MacOS

fg = lambda text, color: "\33[38;5;" + str(color) + "m" + text + "\33[0m"
bg = lambda text, color: "\33[48;5;" + str(color) + "m" + text + "\33[0m"

def print_six(row, format):
    for col in range(6):
        color = row*6 + col + 4
        if color>=0:
            text = "{:3d}".format(color)
            print (format(text,color), end=" ")
        else:
            print("   ", end=" ")

for row in range(-1,42):
    print_six(row, fg)
    print("",end=" ")
    print_six(row, bg)
    print()

# Simple usage: print(fg("text", 160))
```
