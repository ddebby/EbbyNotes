---
title: "Pathlib--Python的路径管理库"
categories:[工具]
tags:[Python,算法]
toc: true
comments: true
date: 2019-05-07 16:53:05
description: "Awesome summary"
comments: true
image: images/fastai_logo.png
---
![Pathlib使用手册](images/fastai_logo.png)

**摘要：** 整理了Pathlib的使用方法，便于速查。

<!-- more-->


## Pathlib使用方法归纳 

### 常用操作


```python
from pathlib import Path
```


```python
p1 = Path()
print(p1)
```

    .


#### 路径


```python
p1.cwd()
```


    PosixPath('/data/home/gaoc/data/1.fast.ai/nbs/dl1')




```python
# 返回一个新的路径，这个新路径就是当前Path对象的绝对路径
p1.resolve()
```


    PosixPath('/data/home/gaoc/data/1.fast.ai/nbs/dl1')




```python
# 也可以获取绝对路径，但是推荐使用resolve()
p1.absolute()
```


    PosixPath('/data/home/gaoc/data/1.fast.ai/nbs/dl1')



#### 获取文件名，文件后缀，文件目录
- name: 目录的最后一个部分
- suffix:目录中最后一个部分的扩展名
- stem：目录最后一个部分，没有后缀
- suffixes：返回多个扩展名列表
- with_suffix(suffix)：补充扩展名到尾部，扩展名存在无效
- with_name(name)：替换目录最后一个部分并返回一个新的路径


```python
p2 = Path('/data/home/gaoc/data/99.test/Untitled.ipynb.test')
p2.name
```


    'Untitled.ipynb'


```python
p2.stem
```


    'Untitled'


```python
p2.suffix
```


    '.ipynb'


```python
p2.suffixes
```


    ['.ipynb', '.test']


```python
p2.parent
```


    PosixPath('/data/home/gaoc/data/99.test')




```python
# 返回一个iterable, 包含所有父目录
for i in p2.parents:
    print(i)
```

    /data/home/gaoc/data/99.test
    /data/home/gaoc/data
    /data/home/gaoc
    /data/home
    /data
    /


### 拼接、检查与分解

操作符：/
Path对象 / Path对象

Path对象 / 字符串 或者 字符串 / Path对象

分解：
parts 属性, 可以返回路径中的每一个部分

joinpath：

joinpath(*other) 连接多个字符串到Path对象中


```python
p3 = Path(p2.parent, 'test.txt')
p3
```


    PosixPath('/data/home/gaoc/data/99.test/test.txt')




```python
p3 = p2.parent / 'test2.txt'
p3
```


    PosixPath('/data/home/gaoc/data/99.test/test2.txt')




```python
p3 = p2.parent.joinpath('etc', 'init.d', Path('abc'))
p3
```


    PosixPath('/data/home/gaoc/data/99.test/etc/init.d/abc')




```python
p3.exists()
```


    False


```python
p3.is_file()
```


    False


```python
p3.is_dir()
```


    False




```python
# 将路径通过分隔符分割成一个元组
p2.parts
```


    ('/', 'data', 'home', 'gaoc', 'data', '99.test', 'Untitled.ipynb.test')



### 遍历文件夹


```python
p4 = p2.parent
p4.iterdir()
```


    <generator object Path.iterdir at 0x7f6dfc0fbeb8>




```python
# 相当于os.listdir
for file in p4.iterdir():
    print(file)
```

    /data/home/gaoc/data/99.test/.ipynb_checkpoints
    /data/home/gaoc/data/99.test/data-science
    /data/home/gaoc/data/99.test/Untitled.ipynb



```python
list(p4.iterdir())
```


    [PosixPath('/data/home/gaoc/data/99.test/.ipynb_checkpoints'),
     PosixPath('/data/home/gaoc/data/99.test/test'),
     PosixPath('/data/home/gaoc/data/99.test/data-science'),
     PosixPath('/data/home/gaoc/data/99.test/Untitled.ipynb')]




```python
# 相当于os.listdir, 但是可以添加匹配条件
for file in p4.glob("*.ipynb"):
    print(file)
```

    /data/home/gaoc/data/99.test/Untitled.ipynb

```python
list(p4.glob("*.ipynb"))
```


    [PosixPath('/data/home/gaoc/data/99.test/Untitled.ipynb')]


```python
# 相当于os.walk, 也可以添加匹配条件
for file in p4.rglob("*.ipynb"):
    print(file)
```

    /data/home/gaoc/data/99.test/Untitled.ipynb
    /data/home/gaoc/data/99.test/.ipynb_checkpoints/Untitled-checkpoint.ipynb

```python
list(p4.rglob("*.ipynb"))
```


    [PosixPath('/data/home/gaoc/data/99.test/Untitled.ipynb'),
     PosixPath('/data/home/gaoc/data/99.test/.ipynb_checkpoints/Untitled-checkpoint.ipynb')]

### 创建文件夹


```python
p5 = Path(p4,'test')
p5.mkdir(exist_ok=True)
```


```python
p5
```


    PosixPath('/data/home/gaoc/data/99.test/test')


```python
p5.mkdir((exist_ok=True, parents=True) # 递归创建文件目录
```

### 查看文件信息

#### 获取详细信息


```python
p6 = Path('/data/home/gaoc/data/99.test/Untitled.ipynb')
p6.stat()
```


    os.stat_result(st_mode=33204, st_ino=57147394, st_dev=2096, st_nlink=1, st_uid=1003, st_gid=1003, st_size=636, st_atime=1555927128, st_mtime=1555926338, st_ctime=1555926338)

#### 文件大小


```python
p6.stat().st_size
```


    636

#### 创建时间


```python
p6.stat().st_atime
```


    1555927128.4478571

#### 修改时间


```python
p6.stat().st_mtime
```


    1555926338.8676176



## 参考

1. <https://www.jianshu.com/p/a820038e65c3>
2. <https://www.cnblogs.com/hkcs/p/7773484.html>