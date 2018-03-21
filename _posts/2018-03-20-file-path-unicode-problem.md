---
layout: post
title: Unicode Problem in a File Path
tags: [Python Unicode]
---

In the last blog, I have talked about the Unicode problem with MySQL database, and how to change the settings to make it support Unicode characters. In this blog, I am going to talk about the Unicode problem in a file path, if we are read/write data with Python 2. This problem is not applicable for Python 3.

Python 2 does not support Unicode encoding by default (Chinese characters, for example), You need to explicitly declare. There are two types encoding for string representation: `str` (default) and `Unicode`, while `Unicode` type is the only one in Python 3. The commonly used Unicode charsets are `utf-8`, `gb2312`, `gbk` etc. Existing encode/decoding conversation functions are：`decode(char_set)` and `Unicodeencode(char_set)`.

For example, assume we have a file ‘测试.txt’, and we would like to read it.

```python
#-*-encoding:utf-8-*-
f = open('D:\\测试.txt')
print f.read()
```

We will encounter the following error:

```python
IOError: [Errno 2] No such file or directory: 'G:\\\xe6\xb5\x8b\xe8\xaf\x95.txt'
```

We need to decode the Unicode file path first,

```python
#-*-encoding:utf-8-*-

f = open('G:\\测试.txt'.decode('utf-8'))
print f.read()
```

For writing a Unicode file with Unicode characters, we have the same problem:

```python
#-*-encoding:utf-8-*-

f = open('G:\\你好.txt', 'w')
f.write("你好")
```

The file name will be unrecognizable： `浣犲ソ.txt`，messed up. We need to change it in this way：

```python
#-*-encoding:utf-8-*-

f = open('G:\\你好.txt'.decode('utf-8'), 'w')
f.write("你好")
```

The output file name and will be normal.

To sum up, we have to be careful dealing with a file path/name with Unicode, under Python 2.x. `decode(char_set)` and `encode(char_set)` are here to solve the problem.

## Reference

- [Unicode HOWTO](https://docs.python.org/2/howto/unicode.html)
