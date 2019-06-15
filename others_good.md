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