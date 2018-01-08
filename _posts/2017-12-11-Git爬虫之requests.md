---
layout:     post
title:        爬虫之requests
subtitle:   requests库
date:       2017-12-11
author:     WSS
header-img: img/post-bg-pandas.jpg
catalog: true
tags:
    - 爬虫
    - requests
---


## requests ##



requests 库有7种方法，不过有一个基本方法，我们可以用`r = requests.get('url') `或`r = requests.request('get','url',**kwargs)`，也就是说，其他的6种方法都是在调用request方法。

在request的**kwargs中，有13个参数可选

![](http://oyug2kd6x.bkt.clouddn.com//PaChong/Requestsrequests7method.png)
	
引入的包：

```python
import requests
```

## 使用 ##



```python

import requests

#(url,params,)
r = requests.get("http://www.baidu.com")#不能是www.baidu.com/baidu.com

print(type(r))
#<class 'requests.models.**Response**'>

#Response属性
print(r.status_code)
#200链接成功，返回成功；404、403……

print(r.text)
#HTTP相应内容的字符串形式，即对url对应的页面内容

print(r.encoding)
#从HTTPheader中猜测的相应内容编码

print(r.content)
#HTTP响应内容的二进制形式



```

![](http://oyug2kd6x.bkt.clouddn.com//PaChong/Requestsrequests流程.png)

