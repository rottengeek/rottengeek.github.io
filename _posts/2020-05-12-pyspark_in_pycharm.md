---
layout: post
title: "在Python中使用远程服务器的pyspark"
author: "Daituodi"
header-style: text
tags:
  - 环境配置
---



>  可能在看到这篇文章以前,你或许一直在使用pip install 的方式来安装pyspark，有时会因为安装版本的不一致导致各种各样的错误发生，这里就来讲述一下Spark 的 python 开发环境搭建(注意前提服务器已经配置好spark环境)。

那么开始我们的正题，如何在本地利用pycharm远程使用服务器的pyspark呢？



## 1- 使用远程服务器的python解释器

在pycharm中进入 Preference -> Project:xxxx 

在Project Interpreter中加入远程解释器地址，配置解释器（Project Interprete）和 路径映射（Path mappings），配置后，我们就可以使用远程的python解释器了，默认是配置后了本地文件和远程文件的自动同步。

![add_interpreter](http://rottengeek.github.io/img/config_env/config_pyspark/add_interpreter.png)

接下来如果我们在PyCharm中输入代码:`import pyspark`

会弹出No module name is ‘pyspark’，我们会发现服务器上 我们使用Python命令行 进入后，`import pyspark`是不会报错的，可是到了本地为什么就报错了呢，是因为服务器上我们配置了pyspark和python解释器的联系，如下图（服务器上/etc/profile，每个的环境配置文件可能不同，如.bash_profile等等）

![etc_profile](http://rottengeek.github.io/img/config_env/config_pyspark/etc_profile.png)

上面几个环境变量是比较重要的。

## 2- 环境变量的配置

（1）点击pycharm工具栏中的Run -> Edit Configurations

![edit_config](http://rottengeek.github.io/img/config_env/config_pyspark/edit_config.png)

配置如上所示的环境变量，具体路径可以根据自己的路径来。

## 3- 解决代码提示的问题

安装上述配置后，我们运行代码是可以了的但是会发现没有pycharm的代码提示，没有这个我还用什么pycharm呀！

现在我们需要把`$SPARK_HOME/python`下的两个必要的目录(就是下图标注的这两个目录) 拷贝在Python安装目录/site-packages这个目录下去，没有`.egg-info`文件也没有关系。

![pyspark_pakage](http://rottengeek.github.io/img/config_env/config_pyspark/pyspark_pakage.png)

如果不知道如何查看python site-packages的同学可以这样查看， 进入python命令行

```python
>>> from distutils.sysconfig import get_python_lib
>>> get_python_lib()
'/usr/local/python3/lib/python3.6/site-packages'
```

至此环境配置就告一段落，我们就可以愉快的使用pycharm进行debug啦。



## 4- 可能会遇到的问题

在配置过程可能会遇到一个其他报错的问题，我简单说明下

- 错误1：Spark：启动pyspark产生NameError: name 'memoryview' is not defined错误的解决方案

```
是因为没有指定python版本环境变量，PYSPARK_PYTHON=python3
```

- 错误2：Exception in thread "main" java.lang.UnsupportedClassVersionError: org/apache/spark/launcher/Main : Unsupported major.minor version 52.0

```
java版本问题或者没有配置java变量
同样可以在Pycharm中配置java环境变量
JAVA_HOME=/usr/lib/jvm/jdk1.8.0_162
```

- 错误3：py4j的问题

```python
原因是spark的python目录下是自带py4j的，但是我们使用的使用没有把py4j的加入到PYTHONPATH中,当然也可以直接在远程服务器直接pip install py4j，因为本质而言py4j就是是一个用 Python 和 Java 编写的库。通过 Py4J，Python程序 能够动态访问Java虚拟机中的Java对象，Java程序也能够回调Python对象。因此使用自己的py4j个spark自带的py4j应该都是没有问题的。
```

