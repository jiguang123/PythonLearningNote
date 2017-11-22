# 搭建TensorFlow环境
## 一、实验介绍
### 1.1 实验内容
TensorFlow 是 Google 开发的一款神经网络的 Python 外部的结构包，也是一个采用 数据流图 来进行数值计算的开源软件库。它被用于语音识别或图像识别等多项机器深度学习领域，它可在小到一部智能手机、大到数千台数据中心服务器的各种设备上运行。

本实验学习 TensorFlow 的基础操作，并用其实现经典的 卷积神经网络 （Convolutional Neural Networks，CNN）。

在本节实验中，我们不会再对 神经网络 以及 卷积神经网络 基本概念作出详细的解释，所以在学习本门课程之前，你需要具备神经网络及卷积神经网的基础， 1.4 先修课程 会帮助你达到这些要求，当然，如果你具备深度学习的基础，那么你可以选择跳过这些课程，直接开始本节实验。那么，现在就让我们一起开启 TensorFlow 的大门，成为一名 “TF-Boy” 吧！

### 1.2 实验知识点
- 数据流图
- TensorFlow基本使用
- 搭建 TensorFlow 环境

### 1.3 实验环境
- python2.7
- TensorFlow
- Xfce终端

## 二、 开始 TensorFlow
### 2.1 为什么学习TensorFlow?
TensorFlow 最初由Google大脑小组（隶属于Google机器智能研究机构）的研究员和工程师们开发出来，用于机器学习和深度神经网络方面的研究，但这个系统的通用性使其也可广泛用于其他计算领域。

TensorFlow 无可厚非地能被认定为神经网络中最好用的库之一，它擅长的任务就是训练深度神经网络。通过使用 TensorFlow 我们就可以快速的入门神经网络，大大降低深度学习（也就是深度神经网络）的开发成本和开发难度；TensorFlow 的开源性让所有人都能使用并且维护、 巩固它，使它能迅速更新, 发展。

虽然可能有些人说 caffe 更适合图像，mxnet 效率更高等等，但其实这些框架一通百通，唯独语法不同而已，所以我们不必在此纠结过多。那么让我们从tensorflow开始吧。

### 2.2 Tensorflow 处理结构
TensorFlow 让我们可以先绘制计算结构图，也可以称是一系列可人机交互的计算操作， 然后把编辑好的Python文件转换成更高效的 C++，并在后端进行计算。

TensorFlow 首先要定义神经网络的结构，然后再把数据放入结构当中去运算和training

下面动图展示了 TensorFlow 数据处理流程：

![image](https://dn-anything-about-doc.qbox.me/document-uid440821labid3267timestamp1500344605144.png/wm)

因为TensorFlow是采用 数据流图（data flow graphs）来计算， 所以首先我们得创建一个数据流图，然后再将我们的数据（数据以 张量(tensor) 的形式存在）放到数据流图中计算。

图中的 节点（Nodes）一般用来表示施加的数学操作，但也可以表示数据输入（feed in）的起点/输出（push out）的终点，或者是读取/写入持久变量（persistent variable）的终点；线（edges）则表示在节点间相互联系的多维数据数组，即 张量（tensor），训练模型时，tensor 会不断的从数据流图中的一个节点 flow 到另一节点, 这就是 TensorFlow 名字的由来。一旦输入端的所有张量准备好，节点将被分配到各种计算设备完成异步并行地执行运算。

它灵活的架构让你可以在多种平台上展开计算，例如台式计算机中的一个或多个CPU（或GPU），服务器，移动设备等等。


### 2.3 Tensorflow 基本使用
使用 TensorFlow，你必须明白 TensorFlow：

- 使用图 (graph) 来表示计算任务
- 在被称之为 会话 (Session) 的上下文 (context) 中执行图
- 使用 tensor 表示数据
- 通过 变量 (Variable) 维护状态
- 使用 feed 和 fetch 可以为任意的操作(arbitrary operation) 赋值或者从其中获取数据


#### 2.3.1 概要
TensorFlow 使用图来表示计算任务，图中的节点被称之为 op (operation 的缩写)。一个 op 获得 0 个或多个 tensor，执行计算产生 0 个或多个 tensor，每个 tensor 是一个类型化的多维数组。例如，你可以将一小组图像集表示为一个四维浮点数数组， 这四个维度分别是 [batch, height, width, channels]。

一个 TensorFlow 图描述了计算的过程。 为了进行计算，图必须在 Session 里被启动。Session 将图的 op 分发到诸如 CPU 或 GPU 之类的设备上，同时提供执行 op 的方法。这些方法执行后，将产生的 tensor 返回。在 Python 语言中, 返回的 tensor 是 numpy ndarray 对象；在 C 和 C++ 语言中，返回的 tensor 是 tensorflow::Tensor 实例。

#### 2.3.2 计算图
TensorFlow 程序通常被组织成一个构建阶段和一个执行阶段。在构建阶段，op 的执行步骤 被描述成一个图。在执行阶段，使用会话执行执行图中的 op。

例如，通常在构建阶段创建一个图来表示和训练神经网络，然后在执行阶段反复执行图中的训练 op。

TensorFlow 支持 C, C++, Python 编程语言。目前，TensorFlow 的 Python 库更加易用， 它提供了大量的辅助函数来简化构建图的工作，这些函数尚未被 C 和 C++ 库支持。

三种语言的 会话库 (session libraries) 是一致的.

#### 2.3.3 构建图
构建图的第一步，是创建源 op (source op)。源 op 不需要任何输入，例如 常量 (Constant)。源 op 的输出被传递给其它 op 做运算。

Python 库中，op 构造器 的返回值代表被构造出的 op 的输出，这些返回值可以传递给其它 op 构造器作为输入。

TensorFlow Python 库有一个默认图 (default graph)，op 构造器可以为其增加节点. 这个默认图对许多程序来说已经足够用了。

```
import tensorflow as tf

# 创建一个常量 op, 产生一个 1x2 矩阵. 这个 op 被作为一个节点
# 加到默认图中.
#
# 构造器的返回值代表该常量 op 的返回值.
matrix1 = tf.constant([[3., 3.]])

# 创建另外一个常量 op, 产生一个 2x1 矩阵.
matrix2 = tf.constant([[2.],[2.]])

# 创建一个矩阵乘法 matmul op , 把 'matrix1' 和 'matrix2' 作为输入.
# 返回值 'product' 代表矩阵乘法的结果.
product = tf.matmul(matrix1, matrix2)
```

默认图现在有三个节点，两个 constant() op，和一个matmul() op。 为了真正进行矩阵相乘运算，并得到矩阵乘法的结果，你必须在会话里启动这个图。

#### 2.3.4 在一个会话中启动图
构造阶段完成后，才能启动图。启动图的第一步是创建一个 Session 对象， 如果无任何创建参数, 会话构造器将启动默认图。

```
# 启动默认图.
sess = tf.Session()

# 调用 sess 的 'run()' 方法来执行矩阵乘法 op, 传入 'product' 作为该方法的参数.
# 上面提到, 'product' 代表了矩阵乘法 op 的输出, 传入它是向方法表明, 我们希望取回
# 矩阵乘法 op 的输出.
#
# 整个执行过程是自动化的, 会话负责传递 op 所需的全部输入. op 通常是并发执行的.
# 
# 函数调用 'run(product)' 触发了图中三个 op (两个常量 op 和一个矩阵乘法 op) 的执行.
#
# 返回值 'result' 是一个 numpy `ndarray` 对象.
result = sess.run(product)
print result
# ==> [[ 12.]]

# 任务完成, 关闭会话.
sess.close()
```

Session 对象在使用完后需要关闭以释放资源. 除了显式调用 close 外, 也可以使用 "with" 代码块 来自动完成关闭动作.


```
with tf.Session() as sess:
  result = sess.run([product])
  print result
```

在实现上，TensorFlow 将图形定义转换成分布式执行的操作，以充分利用可用的计算资源(如 CPU 或 GPU)。一般你不需要显式指定使用 CPU 还是 GPU，TensorFlow 能自动检测。 如果检测到 GPU, TensorFlow 会尽可能地利用找到的第一个 GPU 来执行操作.

如果机器上有超过一个可用的 GPU，除第一个外的其它 GPU 默认是不参与计算的。为了让 TensorFlow 使用这些 GPU，你必须将 op 明确指派给它们执行。

with...Device 语句用来指派特定的 CPU 或 GPU 执行操作：


```
with tf.Session() as sess:
  with tf.device("/gpu:1"):
    matrix1 = tf.constant([[3., 3.]])
    matrix2 = tf.constant([[2.],[2.]])
    product = tf.matmul(matrix1, matrix2)
    ...
```

设备用字符串进行标识. 目前支持的设备包括:


```
"/cpu:0": 机器的 CPU.
"/gpu:0": 机器的第一个 GPU, 如果有的话.
"/gpu:1": 机器的第二个 GPU, 以此类推.
```
当然由于实验环境的限制，我们只能使用 CPU 执行操作。

#### 2.3.5 交互式使用
文档中的 Python 示例使用一个会话 Session 来启动图，并调用 Session.run() 方法执行操作。

为了便于使用诸如 IPython 之类的 Python 交互环境，可以使用 InteractiveSession 代替 Session 类，使用 Tensor.eval() 和 Operation.run() 方法代替 Session.run(). 这样可以避免使用一个变量来持有会话.

```
# 进入一个交互式 TensorFlow 会话.
import tensorflow as tf
sess = tf.InteractiveSession()

x = tf.Variable([1.0, 2.0])
a = tf.constant([3.0, 3.0])

# 使用初始化器 initializer op 的 run() 方法初始化 'x' 
x.initializer.run()

# 增加一个减法 sub op, 从 'x' 减去 'a'. 运行减法 op, 输出结果 
sub = tf.subtract(x, a)
print sub.eval()
# ==> [-2. -1.]
```

#### 2.3.6 Tensor
TensorFlow 程序使用 tensor数据结构来代表所有的数据，计算图中，操作间传递的数据都是 tensor。

张量（tensor)：

- 张量有多种，零阶张量为 纯量或标量 (scalar)，也就是一个数值，比如 [1]
- 一阶张量为 向量 (vector)，比如一维的 [1, 2, 3]
- 二阶张量为 矩阵 (matrix)，比如二维的 [[1, 2, 3],[4, 5, 6],[7, 8, 9]]
- 以此类推, 还有三阶、三维的 …

训练模型时tensor会不断的从数据流图中的一个节点flow到另一节点，这就是TensorFlow名字的由来。

#### 2.3.7 变量
变量维护图执行过程中的状态信息。

下面的例子演示了如何使用变量实现一个简单的计数器。

```
import tensorflow as tf

# 创建一个变量, 初始化为标量 0.
state = tf.Variable(0, name="counter")

# 创建一个 op, 其作用是使 state 增加 1

one = tf.constant(1)
new_value = tf.add(state, one)
update = tf.assign(state, new_value)

# 启动图后, 变量必须先经过`初始化` (init) op 初始化,
# 首先必须增加一个`初始化` op 到图中.
init_op = tf.initialize_all_variables()

# 启动图, 运行 op
with tf.Session() as sess:
  # 运行 'init' op
  sess.run(init_op)
  # 打印 'state' 的初始值
  print sess.run(state)
  # 运行 op, 更新 'state', 并打印 'state'
  for _ in range(3):
    sess.run(update)
    print sess.run(state)

# 输出:

# 0
# 1
# 2
# 3
```

代码中 assign() 操作是图所描绘的表达式的一部分，正如 add() 操作一样。所以在调用 run() 执行表达式之前，它并不会真正执行赋值操作。

通常会将一个统计模型中的参数表示为一组变量，例如，你可以将一个神经网络的权重作为某个变量存储在一个 tensor 中，在训练过程中，通过重复运行训练图，更新这个 tensor。


#### 2.3.8 Fetch
为了取回操作的输出内容，可以在使用 Session 对象的 run() 调用 执行图时，传入一些 tensor，这些 tensor 会帮助你取回结果。

在之前的例子里，我们只取回了单个节点 state，但是你也可以取回多个 tensor：

```
import tensorflow as tf

input1 = tf.constant(3.0)
input2 = tf.constant(2.0)
input3 = tf.constant(5.0)
intermed = tf.add(input2, input3)
mul = tf.multiply(input1, intermed)

with tf.Session() as sess:
  result = sess.run([mul, intermed])
  print result

# 输出:
# [array([ 21.], dtype=float32), array([ 7.], dtype=float32)]
```

需要获取的多个 tensor 值，在 op 的一次运行中一起获得（而不是逐个去获取 tensor）。

#### 2.3.9 Feed
上述示例在计算图中引入了 tensor，以常量或变量的形式存储。TensorFlow 还提供了 feed 机制，该机制可以临时替代图中的任意操作中的 tensor 可以对图中任何操作提交补丁，直接插入一个 tensor。

feed 使用一个 tensor 值临时替换一个操作的输出结果。你可以提供 feed 数据作为 run() 调用的参数。feed 只在调用它的方法内有效，方法结束，feed 就会消失。

最常见的用例是将某些特殊的操作指定为 "feed" 操作，标记的方法是使用 tf.placeholder() 为这些操作创建占位符。


```
import tensorflow as tf

input1 = tf.placeholder(tf.float32)
input2 = tf.placeholder(tf.float32)
output = tf.multiply(input1, input2)

with tf.Session() as sess:
  print sess.run([output], feed_dict={input1:[7.], input2:[2.]})

# 输出:
# [array([ 14.], dtype=float32)]
```

如果没有正确提供 feed，placeholder() 操作将会产生错误。

## 三、基于 VirtualEnv 安装 TensorFlow

    virtualenv 可以创建一个独立的Python运行环境，这样做能使排查安装问题变得更容易。

打开终端

```
$ cd /home/shiyanlou
#安装依赖
$ sudo apt-get update
$ sudo apt-get install python-pip python-dev python-virtualenv
```
接下来，建立一个全新的 virtualenv 环境。为了将环境建在 ~/tensorflow 目录下, 执行:

```
$ virtualenv --system-site-packages ~/tensorflow
$ cd ~/tensorflow
```

然后，激活 virtualenv：

```
$ source bin/activate  # 如果使用 bash
# 终端提示符应该发生变化
```

在 virtualenv 内，安装 TensorFlow：

```
# Ubuntu/Linux 64-bit, CPU only, Python 2.7:
pip install --upgrade https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.8.0-cp27-none-linux_x86_64.whl
```

执行完成，TensorFlow 即成功安装，以后我们每次使用 Tensorflow 的时候，执行如下命令：

```
$ cd ~/tensorflow
$ source bin/activate
```

当使用完 TensorFlow 时，需要关闭环境

```
(tensorflow)$ deactivate  # 停用 virtualenv
$  # 你的命令提示符会恢复原样
```

## 四、参考链接
1. [TensorFlow 英文官方网站](http://tensorflow.org/)
2. [TensorFlow 官方GitHub仓库](https://github.com/tensorflow/tensorflow)














