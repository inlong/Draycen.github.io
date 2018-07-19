---
layout:     post
title:      "python 处理excel"
subtitle:   " \"python\""
date:       2018-07-19 20:00:00
author:     "Draycen"
header-img: "img/post-bg-alitrip.jpg"
catalog: true
tags:
    - python
---

> “python ”

&emsp;&emsp;python 获取excel整列数据。

```python

# -*- coding: utf-8 -*-
 
import pandas as pd
from math import isnan

data = pd.read_excel('jsz.xlsx',skiprows=0)[列标题名称]
print list(data)
print len(list(data))
```









