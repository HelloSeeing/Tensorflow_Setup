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
上面的命令只能下载最新版本的tensorflow，也就是1.1，如果你想要安装一个特定版本的branch（比如说1.0或者release的），可以给```git clone```传入```-b <branchname>```参数（这里我们想要装1.0的话就应该是```-b <r1.0>```）
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

#### Optional: Install CUDA (GPUs on Linux)

In order to build or run TensorFlow with GPU support, both NVIDIA's Cuda Toolkit
(>= 7.0) and cuDNN (>= v3) need to be installed.

TensorFlow GPU support requires having a GPU card with NVidia Compute Capability
(\>= 3.0).  Supported cards include but are not limited to:

* NVidia Titan
* NVidia Titan X
* NVidia K20
* NVidia K40

##### Check NVIDIA Compute Capability of your GPU card

[https://developer.nvidia.com/cuda-gpus](https://developer.nvidia.com/cuda-gpus)

##### Download and install Cuda Toolkit

[https://developer.nvidia.com/cuda-downloads](https://developer.nvidia.com/cuda-downloads)

Install version 8.0 if using our binary releases.

Install the toolkit into e.g. `/usr/local/cuda`.

##### Download and install cuDNN

[https://developer.nvidia.com/cudnn](https://developer.nvidia.com/cudnn)

Download cuDNN v5.1.

Uncompress and copy the cuDNN files into the toolkit directory. Assuming the
toolkit is installed in `/usr/local/cuda`, run the following commands (edited
to reflect the cuDNN version you downloaded):

``` bash
tar xvzf cudnn-8.0-linux-x64-v5.1-ga.tgz
sudo cp -P cuda/include/cudnn.h /usr/local/cuda/include/
sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda/lib64/
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```

##### Install other dependencies

```bash
$ sudo apt-get install libcupti-dev
```

#### Optional: Install OpenCL (Experimental, Linux only)

In order to build or run TensorFlow with OpenCL support, both OpenCL (>= 1.2)
and ComputeCpp (>= 0.1.1) need to be installed.

TensorFlow can only take advantage of accelerators that support OpenCL 1.2.
Supported accelerators include but are not limited to:

*   AMD Fiji
*   AMD Hawaii

Note that this support is currently experimental and should not be relied upon
for production (though it will mature over time).

##### Download and install OpenCL drivers

The exact steps required for a functional OpenCL installation will depend on
your environment. For Unbuntu 14.04, the following steps are known to work:

```bash
sudo apt-get install ocl-icd-opencl-dev opencl-headers
```

You will also need to install the drivers for the accelerator itself. We've
tested that the following drivers for AMD Fiji and Hawaii GPUs on Ubuntu 14.04:

```bash
sudo apt-get install fglrx-core fglrx-dev
```

##### Download and install the ComputeCpp compiler

Download the compiler from [Codeplay's
website](https://www.codeplay.com/products/computesuite/computecpp), uncompress
and copy the files into e.g. `/usr/local/computecpp`:

```bash
tar -xvzf ComputeCpp-CE-0.1.1-Ubuntu.14.04-64bit.tar.gz
sudo mkdir /usr/local/computecpp
sudo cp -R ComputeCpp-CE-0.1.1-Linux /usr/local/computecpp
sudo chmod -R a+r /usr/local/computecpp/
sudo chmod -R a+x /usr/local/computecpp/bin
```
