---
title: json
categories: Python Packages
icon: note
---

python中的json模块用于python对象与json对象的转换及json文件的读写。

## `json.dump`
这个函数主要用于把对象转成str类型并写入json文件。
```python
import json
dict1 = {'a': 1}
with open('a.json', 'w') as f:
    json.dump(dict1, f)
```

## `json.load`
这个函数主要用于从json文件中读取数据并转成python中对应的对象。
```python
import json
with open("a.json", "r") as f:
    dict1 = json.load(f)
    print(dict1)
``` 

## `json.dumps`
这个函数主要用于把python中的对象转成json格式的str，因为如果直接将dict类型的数据写入json文件中会发生报错，因此在将数据写入时需要用到该函数。可以传入indent=2参数，结构更清晰。如果对象中有中文，可以传入ensure_ascii=False参数确保显示正确。
```python
import json
dict1 = {"name": "tom"}
str1 = json.dumps(dict1)
print(type(str1))
with open('a.json', 'w') as f:
    f.write(str1)
```

## `json.loads`
这个函数主要用于把json形式的str转化为python对象。
```python
with open('a.json', 'r') as f:
    str1 = f.read()
dict1 = json.loads(str1)
```