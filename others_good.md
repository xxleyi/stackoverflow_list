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
