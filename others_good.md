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
