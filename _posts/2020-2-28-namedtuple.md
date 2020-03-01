---
layout: post
title: namedtuple module
tags: module python
categories:
 - python
---
### namedtuple

Nice module to improve code readability 

```python
from collections import namedtuple

BuildTime = namedtuple("BT", "name id time")
build = BuildTime("", None, -1)

bt = ["XYZ", 123, "2019-02-04 12:13:01"]
try:
  stat = BuildTime(*bt) # init expects exact number of parameters, expanded by *
except TypeError:
  print("Wrong number of fields")
print(build.name, build.time)
```

https://pymotw.com/2/collections/namedtuple.html

https://pymotw.com/
