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
上面的命令只能下载最新版本的tensorflow，也就是1.1，如果你想要安装一个特定版本的branch（比如说1.0或者release的），可以给```git clone```传入```-b <branchname>```参数（这里我们想要装1.0的话就应该是```-b <r1.0>）
