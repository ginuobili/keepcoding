## 文本和字节序列

#### 1. 字符问题

```python
s = 'café'
s
Out[2]: 'café'

b = s.encode('utf-8')
b
Out[4]: b'caf\xc3\xa9'
b.decode('utf-8')
Out[5]: 'café'
```

.decode() 和 .encode() 的区别，可以把字节序列想成晦涩难懂的机器磁芯转储，把 Unicode 字符串想成“人类可读”的文本。那么，把字节序列变成人类可读的文本字符串就是解码，而把字符串变成用于存储或传输的字节序列就是编码。

####  2. 处理文本

处理文本的最佳实践是“Unicode 三明治” 。意思是，要尽早把输入（例如读取文件时）的字节序列解码成字符串。这种三明治中的“肉片”是程序的业务逻辑， 在这里只能处理字符串对象。在其他处理过程中，一定不能编码或解码。对输出来说，则要尽量晚地把字符串编码成字节序列。多数 Web 框架都是这样做的，使用框架时很少接触字节序列。例如，在 Django 中，视图应该输出 Unicode 字符串；Django 会负责把响应编码成字节序列，而且默认使用 UTF-8 编码。 ![image-20191117141719428](../../zypictures/books/fluentpython4-bytes.png)

在 Python 3 中能轻松地采纳 Unicode 三明治的建议，因为内置的 open 函数会在读取文件时做必要的解码，以文本模式写入文件时还会做必要的编码，所以调用 my_file.read() 方法得到的以及传给 my_file.write(text) 方法的都是字符串对象。

处理文本文件很简单。但是，如果依赖默认编码，会遇到麻烦。需要在多台设备中或多种场合下运行的代码，**一定不能依赖默认编码**。打开文件时始终应该明确传入 encoding= 参数，因为不同的设备使用的默认编码 可能不同，有时隔一天也会发生变化。