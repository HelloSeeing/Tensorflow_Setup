# Tensorflow1.0安装说明
## 环境要求
python需要是2.7或者3.3+

GPU在cuda==8.0, cudnn==5.1的情况下效果最好，其他情况需要从源码安装
## 安装方式
* [使用pip安装](#使用pip安装)
* [从源码安装](#从源码安装)
## 使用pip安装
* 如果要用cpu版本的tensorflow,可以在终端中输入
```bash
$ pip install tensorflow
```
* 如果要用gpu版本的tensorflow,可以在终端中输入
```bash
$ pip install tensorflow-gpu
```
如果上面的命令不管用，可以使用下面的命令进行安装:

```bash
# Ubuntu/Linux 64-bit, CPU only, Python 2.7
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.0.1-cp27-none-linux_x86_64.whl

# Ubuntu/Linux 64-bit, GPU enabled, Python 2.7
# Requires CUDA toolkit 8.0 and CuDNN v5. For other versions, see "Installing from sources" below.
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.0.1-cp27-none-linux_x86_64.whl

# Ubuntu/Linux 64-bit, CPU only, Python 3.3
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.0.1-cp33-cp33m-linux_x86_64.whl

# Ubuntu/Linux 64-bit, GPU enabled, Python 3.3
# Requires CUDA toolkit 8.0 and CuDNN v5. For other versions, see "Installing from sources" below.
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.0.1-cp33-cp33m-linux_x86_64.whl

# Ubuntu/Linux 64-bit, CPU only, Python 3.4
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.0.1-cp34-cp34m-linux_x86_64.whl

# Ubuntu/Linux 64-bit, GPU enabled, Python 3.4
# Requires CUDA toolkit 8.0 and CuDNN v5. For other versions, see "Installing from sources" below.
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.0.1-cp34-cp34m-linux_x86_64.whl

# Ubuntu/Linux 64-bit, CPU only, Python 3.5
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.0.1-cp35-cp35m-linux_x86_64.whl

# Ubuntu/Linux 64-bit, GPU enabled, Python 3.5
# Requires CUDA toolkit 8.0 and CuDNN v5. For other versions, see "Installing from sources" below.
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.0.1-cp35-cp35m-linux_x86_64.whl

# Ubuntu/Linux 64-bit, CPU only, Python 3.6
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.0.1-cp36-cp36m-linux_x86_64.whl

# Ubuntu/Linux 64-bit, GPU enabled, Python 3.6
# Requires CUDA toolkit 8.0 and CuDNN v5. For other versions, see "Installing from sources" below.
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.0.1-cp36-cp36m-linux_x86_64.whl

```

安装Tensorflow:

```bash
# Python 2
$ sudo pip install --upgrade $TF_BINARY_URL

# Python 3
$ sudo pip3 install --upgrade $TF_BINARY_URL
```

注意: 如果你从TensorFlow < 0.7.1 升级到新版本1.0，你应当卸载上一个版本*以及protobuf* 通过`pip uninstall`首先确定你使用了更新过的protobuf依赖项来执行一个干净的安装

## 从源码安装
从源码安装其实是根据自己电脑的配置创建一个pip的wheel文件，再根据```pip install```的方式进行安装，不再赘述
### 从github上下载tensorflow的源码
```bash
$ git clone https://github.com/tensorflow/tensorflow
```
上面的命令只能下载最新版本的tensorflow，也就是1.1，如果你想要安装一个特定版本的branch（比如说1.0或者release的），可以给```git clone```传入```-b <branchname>```参数（这里我们想要装1.0的话就应该是```-b r1.0```）
### 准备工作
#### 安装Bazel

按照[这个网页](http://bazel.build/docs/install.html)的要求去安装bazel的依赖项。然后下载最新稳定版本的bazel通过[适合你系统的安装器](https://github.com/bazelbuild/bazel/releases)并安装网页上面的要求进行安装:

```bash
$ chmod +x PATH_TO_INSTALL.SH
$ ./PATH_TO_INSTALL.SH --user
```

记得将`PATH_TO_INSTALL.SH`换成你下载的安装器路径下的sh文件

最后记得将`bazel`加入你的系统环境目录中去，这个在上面的安装网页中也有写到

#### 安装其他依赖项

```bash
# For Python 2.7:
$ sudo apt-get install python-numpy python-dev python-wheel python-mock
# For Python 3.x:
$ sudo apt-get install python3-numpy python3-dev python3-wheel python3-mock
```

#### 可选: 安装CUDA (GPUs on Linux)

为了去安装GPU版本的Tensorflow, NVIDIA's Cuda Toolkit
(>= 7.0) 和 cuDNN (>= v3)都需要被安装

Tensorflow支持的GPU要求NVidia Compute Capability(\>= 3.0)。比如说:

* NVidia Titan
* NVidia Titan X
* NVidia K20
* NVidia K40

##### 检测你的GPU的NVIDIA Compute Capability

[https://developer.nvidia.com/cuda-gpus](https://developer.nvidia.com/cuda-gpus)

##### 下载并安装Cuda Toolkit

[https://developer.nvidia.com/cuda-downloads](https://developer.nvidia.com/cuda-downloads)

一般来说安装在`/usr/local/cuda`.

##### 下载并安装cudnn

[https://developer.nvidia.com/cudnn](https://developer.nvidia.com/cudnn)

下载cuDNN v5.1.

注意，这其中会要求你注册NVIDIA账号，才能够下载cudnn

下载好后解压并将cuDNN的文件拷入到你的cuda目录里面去。假设你的cuda安装在`/usr/local/cuda`，运行下面的命令(你运行时可能因为你cuda或者cudnn的版本不同而稍有不同):

``` bash
tar xvzf cudnn-8.0-linux-x64-v5.1-ga.tgz
sudo cp -P cuda/include/cudnn.h /usr/local/cuda/include/
sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda/lib64/
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```

##### 安装其他依赖项

```bash
$ sudo apt-get install libcupti-dev
```

### 构建Tensorflow
在下载Tensorflow的根目录下运行`configure`。程序会询问你是否要Tensorflow支持这个，支持那个，支持则输入y，不支持输入n。在这里我们只考虑GPU的安装支持，如果你想要安装GPU版本的话，在```Do you wish to build Tensorflow with GPU support ? [y/N]```后面输入```y```即可。后面会要求你输入你系统里安装的cuda和cudnn版本，如果你安装了多个版本的cuda或者cudnn，那么就得输入特定的版本，而不要去使用默认值。

例如你可以这样选择:

```bash
$ ./configure
Please specify the location of python. [Default is /usr/bin/python]:
Do you wish to build TensorFlow with Google Cloud Platform support? [y/N] N
No Google Cloud Platform support will be enabled for TensorFlow
Do you wish to build TensorFlow with GPU support? [y/N] y
Do you wish to build TensorFlow with OpenCL support? [y/N] N
GPU support will be enabled for TensorFlow
Please specify which gcc nvcc should use as the host compiler. [Default is /usr/bin/gcc]:
Please specify the Cuda SDK version you want to use, e.g. 7.0. [Leave empty to use system default]: 8.0
Please specify the location where CUDA 8.0 toolkit is installed. Refer to README.md for more details. [Default is /usr/local/cuda]:
Please specify the cuDNN version you want to use. [Leave empty to use system default]: 5
Please specify the location where cuDNN 5 library is installed. Refer to README.md for more details. [Default is /usr/local/cuda]:
Please specify a list of comma-separated Cuda compute capabilities you want to build with.
You can find the compute capability of your device at: https://developer.nvidia.com/cuda-gpus.
Please note that each additional compute capability significantly increases your build time and binary size.

Setting up Cuda include
Setting up Cuda lib
Setting up Cuda bin
Setting up Cuda nvvm
Setting up CUPTI include
Setting up CUPTI lib64
Configuration finished
```

[Default is: "3.5,5.2"]: 3.0

如果以后你要重新安装Tensorflow的话，要先运行```bazel clean```，再运行这个```configure```

### 构建pip的安装包并安装
上一步完成之后，还要构建pip的安装包

注意到如果使用默认参数的话这个过程会占用巨量的内存，如果你希望限制内存使用量的话你可以使用`--local_resources 2048,.5,1.0`参数。

```bash
$ bazel build -c opt //tensorflow/tools/pip_package:build_pip_package

# To build with support for CUDA:
$ bazel build -c opt --config=cuda //tensorflow/tools/pip_package:build_pip_package

$ bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg

# The name of the .whl file will depend on your platform.
$ sudo pip install /tmp/tensorflow_pkg/tensorflow-1.0.1-py2-none-any.whl
```

## 优化CPU

为了尽可能的和最多的电脑适配, TensorFlow默认使用SSE4.1 SIMD instructions在x86机器上。然而现代大部分的电脑支持更高级的instructions, 因此如果你希望只在你电脑上运行的话,可以通过给```bazel```命令添加`--copt=-march=native`参数:

``` bash
$ bazel build --copt=-march=native -c opt //tensorflow/tools/pip_package:build_pip_package
```

### 测试你的Tensorflow是否安装成功
```bash
$ python
...
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
