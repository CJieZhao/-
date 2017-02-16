# TensorFlow
---

## windows 安装(Python pip 64位)
[参考](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/g3doc/get_started/os_setup.md#pip-installation-on-windows)

1. 安装python3.5
1. 安装依赖环境[Visual C++ 2015 redistributable](https://www.microsoft.com/en-us/download/details.aspx?id=53587)
1. 执行安装命令
	* cpu 版: `C:\> pip install --upgrade https://storage.googleapis.com/tensorflow/windows/cpu/tensorflow-1.0.0rc2-cp35-cp35m-win_amd64.whl`
	* gpu 版: `C:\> pip install --upgrade https://storage.googleapis.com/tensorflow/windows/gpu/tensorflow_gpu-1.0.0rc2-cp35-cp35m-win_amd64.whl`

1. 测试安装情况:

```
$ python
>>> import tensorflow as tf
>>> hello = tf.constant('Hello, TensorFlow!')
>>> sess = tf.Session()
>>> print(sess.run(hello))
Hello, TensorFlow!
>>> a = tf.constant(10)
>>> b = tf.constant(32)
>>> print(sess.run(a + b))
42
>>>
```