# python2 中文乱码

 str = '\\u5927\\u77f3\\u8857\\u9053\\u690d\\u6751\\u4e09\\u8def\\u56db\\u5df79\\u53f7'

使用：str.encode('utf-8').decode('unicode_escape')

```python
str.encode('utf-8').decode('unicode_escape')
```

