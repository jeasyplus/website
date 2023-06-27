# TensorFlow 简介

TensorFlow 是一个端到端开源机器学习平台。TensorFlow 是一个用于管理机器学习系统各个方面的丰富系统；

## TensorFlow API 

主要介绍如何使用特定的api，来开发和训练机器学习模型。

TensorFlow API 按层级排列，高阶 API 基于低阶 API 构建。

机器学习研究人员会使用低级别 API 来创建和探索新的机器学习算法。

在本课程中，将使用名为 tf.keras 的高阶 API 来定义和训练机器学习模型，并进行预测。

tf.keras 是开源 Keras API 的 TensorFlow 变体。

下图显示了 TensorFlow 工具包的层次结构：

![图](https://jeasyplus.com/images/machine-learning/2023-06-26-21.41.41.png)

__图 1. TensorFlow 工具包层次结构。__


### 扩展阅读
TPU（Tensor Processing Unit）是由Google设计和开发的专用集成电路（ASIC），旨在加速人工智能（AI）工作负载的处理速度。它专门针对深度学习任务进行了优化，特别适用于大规模的神经网络计算。

与通用的中央处理单元（CPU）和图形处理单元（GPU）不同，TPU专注于进行张量（Tensor）操作，这是深度学习中常见的数据结构。TPU的硬件架构和特殊设计使其能够高效地执行大规模矩阵运算和张量操作，从而提供了卓越的计算性能和能效。

### TPU的主要优势包括：

+ 高速计算能力：TPU可以在深度学习任务中提供比传统CPU和GPU更高的计算性能。它能够以更快的速度执行大规模的矩阵乘法、卷积和其他深度学习操作。


+ 高效能耗比：TPU在相同的功耗下可以完成更多的计算任务，具有更高的能效性能。这使得在大规模训练和推理任务中能够更加经济地使用计算资源。


+ 专用优化：TPU的架构和指令集专门针对深度学习任务进行了优化，使其能够高效地执行常见的深度学习操作，提供较低的延迟和较高的吞吐量。


+ Google在其云计算平台Google Cloud上提供了TPU的访问和使用，使开发者能够利用TPU的强大计算能力来加速他们的深度学习工作负载。TPU的出现进一步推动了深度学习和人工智能技术的发展，为解决更复杂和大规模的问题提供了更强大的计算能力。

## 编程练习

将通过在 tf.keras 中对模型进行编码来实际运用机器学习概念。

您将使用 Colab 作为编程环境。Colab 是 Google 的 Jupyter 笔记本版本。与 Jupyter 笔记本一样，Colab 提供了一个将文本、代码、图形和程序输出组合在一起的交互式 Python 编程环境。

### NumPy 和 Pandas

使用 tf.keras 时，您至少需要对以下两个开源 Python 库有所了解：

NumPy（Numerical Python）和Pandas是两个在数据科学和分析领域广泛使用的Python库。

+ [NumPy](https://numpy.org/) - 简化对数组的表示和执行线性代数运算。
  + NumPy是Python中用于科学计算和数值操作的基础库。
  + 它提供了高性能的多维数组（ndarray）对象，以及对数组进行操作和计算的函数。
  + NumPy的核心功能包括对数组进行索引、切片、排序、统计计算等。
  + 它还提供了线性代数、傅里叶变换、随机数生成等功能。
  + NumPy的优势在于其高效的数组操作和广播功能，使得处理大规模数据集更加高效和便捷。

```python
# 导入NumPy的标准方式：
import numpy as np

# 创建一个二维数组，设置某些行的每隔一个元素为-99，并找到每行的最大值：

x = np.arange(15, dtype=np.int64).reshape(3, 5)
x[1:, ::2] = -99
x
# array([[  0,   1,   2,   3,   4],
#        [-99,   6, -99,   8, -99],
#        [-99,  11, -99,  13, -99]])

x.max(axis=1)
# array([ 4,  8, 13])

# 生成正态分布的随机数：
rng = np.random.default_rng()
samples = rng.normal(size=2500)
samples
```

+ [pandas](https://pandas.pydata.org/)，它提供了一种表示内存中数据集的简单方法。
  + Pandas是建立在NumPy之上的数据处理和分析库，提供了高效、灵活且易于使用的数据结构和工具。
  + 它的核心数据结构是Series和DataFrame。
  + Series是一维标记数组，类似于带有标签的一维数组或列。
  + DataFrame是具有行和列的二维标记表格，类似于数据库表或Excel电子表格。
  + Pandas提供了各种数据操作和处理功能，包括数据的读取和写入、数据清洗和转换、数据筛选和排序、数据聚合和分组等。
  + 它还具备对缺失数据的处理、时间序列数据的处理、数据的合并和连接、数据的可视化等功能。
  + Pandas的优势在于其灵活性和高效性，能够处理不规则和缺失数据，并提供了广泛的数据操作和分析工具，使得数据科学工作更加便捷和高效。


```python
In [1]: import pandas as pd
```

```python
In [2]: df = pd.DataFrame(
   ...:     {
   ...:         "Name": [
   ...:             "Braund, Mr. Owen Harris",
   ...:             "Allen, Mr. William Henry",
   ...:             "Bonnell, Miss. Elizabeth",
   ...:         ],
   ...:         "Age": [22, 35, 58],
   ...:         "Sex": ["male", "male", "female"],
   ...:     }
   ...: )
   ...: 

In [3]: df
Out[3]: 
                       Name  Age     Sex
0   Braund, Mr. Owen Harris   22    male
1  Allen, Mr. William Henry   35    male
2  Bonnell, Miss. Elizabeth   58  female
```

![图](https://jeasyplus.com/images/machine-learning/2023-06-26-21.59.42.png)


综上所述，NumPy和Pandas是两个重要的Python库，NumPy主要用于数值计算和数组操作，而Pandas则专注于数据处理和分析。

它们在数据科学和机器学习领域扮演着重要角色，并经常与其他库（如Matplotlib和Scikit-learn）一起使用，构建完整的数据分析和机器学习工作流程。


## 使用 tf.keras 进行线性回归

1.使用合成数据进行线性回归 Colab 练习：使用玩具数据集探索线性回归。

2.使用实际数据集进行线性回归 Colab 练习，它会引导您在真实数据集上进行各种分析。

## 使用合成数据进行简单线性回归
在这个第一个Colab中，你将使用一个简单的数据库探索线性回归。


### 学习目标：
__调整以下超参数：__
+ 学习率
+ 迭代次数
+ 批量大小

__解读不同类型的损失曲线。__

```python
import pandas as pd
import tensorflow as tf
from matplotlib import pyplot as plt
```
__定义构建和训练模型的函数__

以下代码定义了两个函数：

build_model(my_learning_rate)：用于构建一个空模型。

train_model(model, feature, label, epochs)：用于根据传入的示例（feature和label）训练模型。

```python
#@title 定义构建和训练模型的函数

def build_model(my_learning_rate):
  """创建并编译一个简单的线性回归模型。"""
  # 大多数简单的 tf.keras 模型是顺序模型。顺序模型包含一个或多个层。
  model = tf.keras.models.Sequential()

  # 描述模型的拓扑结构。
  # 简单线性回归模型的拓扑结构是一个单层中的单个节点。
  model.add(tf.keras.layers.Dense(units=1, input_shape=(1,)))

  # 将模型的拓扑结构编译为 TensorFlow 可以高效执行的代码。
  # 配置训练以最小化模型的均方误差。
  model.compile(optimizer=tf.keras.optimizers.experimental.RMSprop(learning_rate=my_learning_rate),
                loss="mean_squared_error",
                metrics=[tf.keras.metrics.RootMeanSquaredError()])

  return model

def train_model(model, feature, label, epochs, batch_size):
  """通过提供数据来训练模型。"""

  # 将特征值和标签值提供给模型。
  # 模型将在指定的 epochs 数量内进行训练，逐渐学习特征值与标签值之间的关系。
  history = model.fit(x=feature,
                      y=label,
                      batch_size=batch_size,
                      epochs=epochs)

  # 获取训练好的模型的权重和偏置。
  trained_weight = model.get_weights()[0]
  trained_bias = model.get_weights()[1]

  # 将 epochs 的列表与其余的历史记录分开存储。
  epochs = history.epoch

  # 收集每个 epoch 的历史记录（快照）。
  hist = pd.DataFrame(history.history)

  # 特别收集模型在每个 epoch 的均方根误差。
  rmse = hist["root_mean_squared_error"]

  return trained_weight, trained_bias, epochs, rmse

print("Defined the build_model and train_model functions.")
```

### 定义绘图函数
我们使用一个流行的Python库Matplotlib来创建以下两个图表：

+ 特征值与标签值的图表，以及显示训练模型输出的线条。


+ 损失曲线。

```python
#@title 定义绘图函数

def plot_the_model(trained_weight, trained_bias, feature, label):
  """绘制训练好的模型与训练特征值和标签值之间的关系图。"""

  # 标记坐标轴。
  plt.xlabel("feature")
  plt.ylabel("label")

  # 绘制特征值与标签值的散点图。
  plt.scatter(feature, label)

  # 创建表示模型的红色线条。红色线条的起点为坐标（x0，y0），终点为坐标（x1，y1）。
  x0 = 0
  y0 = trained_bias
  x1 = feature[-1]
  y1 = trained_bias + (trained_weight * x1)
  plt.plot([x0, x1], [y0, y1], c='r')

  # 显示散点图和红色线条。
  plt.show()

def plot_the_loss_curve(epochs, rmse):
  """绘制损失曲线，显示损失与迭代轮数的关系。"""

  plt.figure()
  plt.xlabel("迭代轮数")
  plt.ylabel("均方根误差")

  plt.plot(epochs, rmse, label="损失")
  plt.legend()
  plt.ylim([rmse.min()*0.97, rmse.max()])
  plt.show()

print("Defined the plot_the_model and plot_the_loss_curve functions")
```

### 定义数据集
该数据集包含12个示例。每个示例包含一个 __feature__ 和一个 __label__。
```python
my_feature = ([1.0, 2.0,  3.0,  4.0,  5.0,  6.0,  7.0,  8.0,  9.0, 10.0, 11.0, 12.0])
my_label   = ([5.0, 8.8,  9.6, 14.2, 18.8, 19.5, 21.4, 26.8, 28.9, 32.0, 33.8, 38.2])
```

### 指定超参数
在这个Colab中，超参数如下：
+ 学习率（learning rate）
+ 迭代轮数（epochs）
+ 批次大小（batch_size）

以下代码单元初始化这些超参数，然后调用构建和训练模型的函数。

```python
learning_rate=0.01
epochs=10
my_batch_size=12

my_model = build_model(learning_rate)
trained_weight, trained_bias, epochs, rmse = train_model(my_model, my_feature, 
                                                         my_label, epochs,
                                                         my_batch_size)
plot_the_model(trained_weight, trained_bias, my_feature, my_label)
plot_the_loss_curve(epochs, rmse)
```
```
Epoch 1/10
1/1 [==============================] - 0s 493ms/step - loss: 1114.9135 - root_mean_squared_error: 33.3903
Epoch 2/10
1/1 [==============================] - 0s 17ms/step - loss: 1097.5588 - root_mean_squared_error: 33.1294
Epoch 3/10
1/1 [==============================] - 0s 21ms/step - loss: 1085.1005 - root_mean_squared_error: 32.9409
Epoch 4/10
1/1 [==============================] - 0s 12ms/step - loss: 1074.7465 - root_mean_squared_error: 32.7833
Epoch 5/10
1/1 [==============================] - 0s 20ms/step - loss: 1065.6124 - root_mean_squared_error: 32.6437
Epoch 6/10
1/1 [==============================] - 0s 23ms/step - loss: 1057.2880 - root_mean_squared_error: 32.5160
Epoch 7/10
1/1 [==============================] - 0s 17ms/step - loss: 1049.5442 - root_mean_squared_error: 32.3967
Epoch 8/10
1/1 [==============================] - 0s 13ms/step - loss: 1042.2396 - root_mean_squared_error: 32.2837
Epoch 9/10
1/1 [==============================] - 0s 14ms/step - loss: 1035.2787 - root_mean_squared_error: 32.1757
Epoch 10/10
1/1 [==============================] - 0s 11ms/step - loss: 1028.5946 - root_mean_squared_error: 32.0717
```
![图](https://jeasyplus.com/images/machine-learning/root_meta_s_e_2023_06_26_01.png)
![图](https://jeasyplus.com/images/machine-learning/root_meta_s_e_2023_06_26_02.png)

**任务1：检查图形**

检查顶部的图表。蓝色的点表示实际数据，红色的线表示训练模型的输出。理想情况下，红色的线应该与蓝色的点很好地对齐。是这样吗？可能不是。

训练模型时会有一定的随机性，所以每次训练都会得到略有不同的结果。也就是说，除非你是一个非常幸运的人，否则红色的线可能不会与蓝色的点很好地对齐。

检查底部的图表，它显示了**损失曲线**。注意**损失曲线**的下降趋势，但没有趋于平缓，这表明模型尚未充分训练。


**任务2：增加迭代轮数**

训练损失应该逐渐降低，一开始降低得很快，然后逐渐减缓。最终，训练损失应该保持稳定（斜率为零或接近零），这表示训练已经收敛。

在**任务1**中，训练损失没有收敛。一个可能的解决方案是增加**迭代轮数**。你的任务是增加足够多的**迭代轮数**，使模型收敛。然而，过度训练也是低效的，所以不要将**迭代轮数**设置为任意的高值。

检查**损失曲线**。模型是否收敛？
```python
learning_rate=0.01
epochs= 450   # 将 ? 替换为一个整数。
my_batch_size=12

my_model = build_model(learning_rate)
trained_weight, trained_bias, epochs, rmse = train_model(my_model, my_feature, 
                                                        my_label, epochs,
                                                        my_batch_size)
plot_the_model(trained_weight, trained_bias, my_feature, my_label)
plot_the_loss_curve(epochs, rmse)
```
经过将 **迭代轮数（epochs）** 调整到450后，得到如下图：
![图](https://jeasyplus.com/images/machine-learning/root_meta_s_e_2023_06_26_03.png)
![图](https://jeasyplus.com/images/machine-learning/root_meta_s_e_2023_06_26_05.png)

**任务3：增加学习率**

在**任务2**中，你增加了**迭代轮数**以使模型收敛。

有时，通过增加学习率可以更快地使模型收敛。然而，将**学习率**设置得太高往往会使模型无法收敛。

在**任务3**中，我们故意将**学习率**设置得过高。运行下面的代码单元格，看看会发生什么。
```python
# 增加学习率并减少迭代轮数
learning_rate=100 
epochs=500 

my_model = build_model(learning_rate)
trained_weight, trained_bias, epochs, rmse = train_model(my_model, my_feature, 
                                                         my_label, epochs,
                                                         my_batch_size)
plot_the_model(trained_weight, trained_bias, my_feature, my_label)
plot_the_loss_curve(epochs, rmse)
```
调整后得到图形如下：

![图](https://jeasyplus.com/images/machine-learning/root_meta_s_e_2023_06_26_07.png)
![图](https://jeasyplus.com/images/machine-learning/root_meta_s_e_2023_06_26_08.png)

结果的模型很糟糕；

红色的线与蓝色的点不对齐。此外，损失曲线像过山车一样震荡。震荡的**损失曲线**强烈表明学习率过高。

**任务4：找到迭代轮数和学习率的理想组合**

为了使训练收敛尽可能高效，请为以下两个超参数分配合适的值：
+ 学习率（learning_rate）


+ 迭代轮数（epochs）


```python
#设置学习率和迭代轮数

learning_rate=0.14
epochs=70

my_model = build_model(learning_rate)
trained_weight, trained_bias, epochs, rmse = train_model(my_model, my_feature, 
                                                         my_label, epochs,
                                                         my_batch_size)
plot_the_model(trained_weight, trained_bias, my_feature, my_label)
plot_the_loss_curve(epochs, rmse)
```
调整后图形如下：

![图](https://jeasyplus.com/images/machine-learning/root_meta_s_e_2023_06_26_010.png)
![图](https://jeasyplus.com/images/machine-learning/root_meta_s_e_2023_06_26_011.png)

**任务5：调整批次大小**

系统在每次迭代之后重新计算模型的**损失值**，并调整模型的**权重**和**偏置**。每次迭代是系统处理一个批次的范围。

例如，如果批次大小为6，则系统在处理每6个示例之后重新计算模型的**损失值**，并调整模型的**权重**和**偏置**。

一个**迭代轮数**足够处理数据集中的每个示例。

例如，如果批次大小为12，则每个迭代轮数持续一次迭代。然而，如果批次大小为6，则每个迭代轮数需要两次迭代。

诱人的做法是将批次大小直接设置为数据集中的示例数量（在这种情况下为12）。然而，模型在较小的批次上可能实际上训练得更快。相反，非常小的批次可能不包含足够的信息帮助模型收敛。

在下面的代码单元格中尝试不同的 batch_size。在模型在一百个迭代轮数内收敛的情况下，您可以将 batch_size 设置为最小的整数是多少？

```python
learning_rate=0.05
epochs=125
my_batch_size=1 # 哇，批次大小为1可以工作！

my_model = build_model(learning_rate)
trained_weight, trained_bias, epochs, rmse = train_model(my_model, my_feature, 
                                                         my_label, epochs,
                                                         my_batch_size)
plot_the_model(trained_weight, trained_bias, my_feature, my_label)
plot_the_loss_curve(epochs, rmse)
```
调整后图形如下：

![图](https://jeasyplus.com/images/machine-learning/root_meta_s_e_2023_06_26_013.png)
![图](https://jeasyplus.com/images/machine-learning/root_meta_s_e_2023_06_26_015.png)

## 总结
### 超参数调优总结
大多数机器学习问题需要进行大量的超参数调优。不幸的是，我们无法为每个模型提供具体的调优规则。

降低**学习率**可以帮助某个模型高效收敛，但可能会导致另一个模型收敛过慢。您必须进行实验，找到适合您数据集的最佳超参数组合。以下是一些经验法则：

+ 训练损失应该稳步下降，一开始下降得较快，然后变得更慢，直到**曲线的斜率**达到或接近零。
+ 如果**训练损失**没有收敛，增加**训练轮数**。
+ 如果**训练损失**下降得太慢，增加**学习率**。请注意，将**学习率**设置得太高也可能阻止**训练损失**的收敛。
+ 如果**训练损失**波动很大（即训练损失跳动不稳定），降低**学习率**。
+ 降低**学习率**的同时增加**训练轮数**或**批次大小**通常是一个不错的组合。
+ 将**批次大小**设置为非常小的值可能会导致不稳定性。首先，尝试较大的批次大小值。然后，逐渐减小批次大小，直到出现性能下降。
+ 对于包含大量示例的实际数据集，整个数据集可能无法一次性装入内存。在这种情况下，您需要减小批次大小，以使一个批次能够适应内存中。

注意：以上翻译的是原文中的一般规则和经验，实际调优过程仍需根据具体情况进行实验和探索。

## 使用合成数据进行简单线性回归
在这个Colab中，将使用一个简单的数据集来探索线性回归。

学习目标：
+ 将.csv文件读入pandas DataFrame。
+ 查看数据集。
+ 尝试使用不同的特征构建模型。
+ 调整模型的**超参数**。

**数据集：**
本次练习使用的数据集基于加利福尼亚州1990年的人口普查数据。

虽然数据集比较旧，但仍然提供了学习机器学习编程的绝佳机会。

**导入相关模块**
```python
#@title 导入相关模块
import pandas as pd
import tensorflow as tf
from matplotlib import pyplot as plt

# 下面的代码用于调整报告的粒度。
pd.options.display.max_rows = 10
pd.options.display.float_format = "{:.1f}".format
```

### 数据集
数据集通常以.csv格式存储在磁盘上或者通过URL获取。

一个格式良好的.csv文件包含第一行的列名，后面跟着许多行的数据。每行中的每个值之间用逗号分隔。

例如，下面是包含加利福尼亚房屋数据集的.csv文件的前五行：

```
"longitude","latitude","housing_median_age","total_rooms","total_bedrooms","population","households","median_income","median_house_value"
-114.310000,34.190000,15.000000,5612.000000,1283.000000,1015.000000,472.000000,1.493600,66900.000000
-114.470000,34.400000,19.000000,7650.000000,1901.000000,1129.000000,463.000000,1.820000,80100.000000
-114.560000,33.690000,17.000000,720.000000,174.000000,333.000000,117.000000,1.650900,85700.000000
-114.570000,33.640000,14.000000,1501.000000,337.000000,515.000000,226.000000,3.191700,73400.000000
```

### 加载 .csv 文件到 pandas DataFrame
这个 Colab，和许多机器学习程序一样，会将 .csv 文件读取并将数据存储在内存中，以 pandas DataFrame 的形式。

Pandas 是一个开源的 Python 库，它的主要数据类型是 DataFrame。你可以将 pandas DataFrame 想象成一个电子表格，其中每一行都有一个编号，每一列都有一个名称。

Pandas 本身是建立在另一个开源的 Python 库 NumPy 的基础上的。如果你对这些技术不熟悉，请查看下面这两个简短的教程：

+ NumPy
+ Pandas DataFrame

下面的代码单元格将 .csv 文件导入到 pandas DataFrame 中，并对标签（median_house_value）中的值进行了缩放：
```python
# 导入数据集。
training_df = pd.read_csv(filepath_or_buffer="https://download.mlcc.google.com/mledu-datasets/california_housing_train.csv")

# 缩放标签值。
training_df["median_house_value"] /= 1000.0

# 打印 pandas DataFrame 的前几行数据。
training_df.head()
```

```python
| longitude | latitude | housing_median_age | total_rooms | total_bedrooms | population | households | median_income | median_house_value |
|-----------|----------|--------------------|-------------|----------------|------------|------------|---------------|-------------------|
| -114.3    | 34.2     | 15.0               | 5612.0      | 1283.0         | 1015.0     | 472.0      | 1.5           | 66.9              |
| -114.5    | 34.4     | 19.0               | 7650.0      | 1901.0         | 1129.0     | 463.0      | 1.8           | 80.1              |
| -114.6    | 33.7     | 17.0               | 720.0       | 174.0          | 333.0      | 117.0      | 1.7           | 85.7              |
| -114.6    | 33.6     | 14.0               | 1501.0      | 337.0          | 515.0      | 226.0      | 3.2           | 73.4              |
| -114.6    | 33.6     | 20.0               | 1454.0      | 326.0          | 624.0      | 262.0      | 1.9           | 65.5              |
```

对 median_house_value 进行缩放将每个房屋的值以千为单位进行表示。

缩放可以将损失值和学习率保持在更友好的范围内。

尽管通常情况下对**标签**进行缩放并不是必要的，但在**多特征模型**中进行特征**缩放通常是必要**的。

### 检查数据集
在大多数机器学习项目中，了解数据是非常重要的。pandas API 提供了一个 describe 函数，用于输出 DataFrame 中每列的以下统计信息：

+ count，即该列中的行数。理想情况下，每列的 count 值应相同。

+ mean 和 std，分别表示每列中值的平均值和标准差。

+ min 和 max，分别表示每列中的最小值和最大值。

+ 25%，50%，75%，分别表示各种分位数值。
```python
# 获取数据集的统计信息
training_df.describe()
```

```python
| Statistic | longitude | latitude | housing_median_age | total_rooms | total_bedrooms | population | households | median_income | median_house_value |
|-----------|-----------|----------|--------------------|-------------|----------------|------------|------------|---------------|--------------------|
| count     | 17000.0   | 17000.0  | 17000.0            | 17000.0     | 17000.0        | 17000.0    | 17000.0    | 17000.0       | 17000.0            |
| mean      | -119.6    | 35.6     | 28.6               | 2643.7      | 539.4          | 1429.6     | 501.2      | 3.9           | 207.3              |
| std       | 2.0       | 2.1      | 12.6               | 2179.9      | 421.5          | 1147.9     | 384.5      | 1.9           | 116.0              |
| min       | -124.3    | 32.5     | 1.0                | 2.0         | 1.0            | 3.0        | 1.0        | 0.5           | 15.0               |
| 25%       | -121.8    | 33.9     | 18.0               | 1462.0      | 297.0          | 790.0      | 282.0      | 2.6           | 119.4              |
| 50%       | -118.5    | 34.2     | 29.0               | 2127.0      | 434.0          | 1167.0     | 409.0      | 3.5           | 180.4              |
| 75%       | -118.0    | 37.7     | 37.0               | 3151.2      | 648.2          | 1721.0     | 605.2      | 4.8           | 265.0              |
| max       | -114.3    | 42.0     | 52.0               | 37937.0     | 6445.0         | 35682.0    | 6082.0     | 15.0          | 500.0              |
```

**任务 1：识别数据集中的异常值**

你是否在数据中看到任何异常值（奇怪的数值）？
```python
"""
# 几个列的最大值（max）与其他四分位数相比似乎非常高。例如，total_rooms列。
# 根据四分位数值（25％，50％和75％），您可能期望total_rooms的最大值约为5,000或可能为10,000。然而，实际的最大值却是37,937。

# 当您在一列中看到异常值时，要更加谨慎地将该列作为特征使用。
# 尽管如此，潜在特征中的异常值有时会与标签中的异常值相对应，这可能使该列成为一个（或看起来是）强有力的特征。
# 此外，正如您在课程后面将会看到的，您可以通过表示（预处理）原始数据来将列转化为有用的特征。
"""
```

**定义构建和训练模型的函数**

下面的代码定义了两个函数：

+ build_model(my_learning_rate)：用于构建一个随机初始化的模型。


+ train_model(model, feature, label, epochs)：用于根据传入的示例（feature和label）训练模型。

由于您现在不需要理解构建模型的代码，我们已将此代码单元格隐藏起来。您可以选择双击以下标题以查看构建和训练模型的代码。

__定义构建和训练模型的函数__
```python
#@title 定义构建和训练模型的函数
def build_model(my_learning_rate):
  """创建并编译一个简单的线性回归模型。"""
  # 大多数简单的tf.keras模型是顺序模型。
  model = tf.keras.models.Sequential()

  # 描述模型的拓扑结构。
  # 简单线性回归模型的拓扑结构是单个层中的单个节点。
  model.add(tf.keras.layers.Dense(units=1, 
                                  input_shape=(1,)))

  # 将模型的拓扑结构编译成TensorFlow可以高效执行的代码。配置训练以最小化模型的均方误差。
  model.compile(optimizer=tf.keras.optimizers.experimental.RMSprop(learning_rate=my_learning_rate),
                loss="mean_squared_error",
                metrics=[tf.keras.metrics.RootMeanSquaredError()])

  return model        


def train_model(model, df, feature, label, epochs, batch_size):
  """通过提供数据来训练模型。"""

  # 将特征和标签提供给模型。
  # 模型将在指定的epoch数上进行训练。
  history = model.fit(x=df[feature],
                      y=df[label],
                      batch_size=batch_size,
                      epochs=epochs)

  # 收集训练模型的权重和偏差。
  trained_weight = model.get_weights()[0]
  trained_bias = model.get_weights()[1]

  # epoch列表与其余的history分开存储。
  epochs = history.epoch
  
  # 提取每个epoch的误差。
  hist = pd.DataFrame(history.history)

  # 为了跟踪训练的进展，我们将在每个epoch处记录模型的均方根误差。
  rmse = hist["root_mean_squared_error"]

  return trained_weight, trained_bias, epochs, rmse

print("已定义build_model和train_model函数。")
```

**定义绘图函数**

以下的matplotlib函数创建了以下图表：

+ 一个特征与标签的散点图，并显示经过训练的模型的输出线条

+ 一个损失曲线

您可以选择双击标题以查看matplotlib代码，但请注意编写matplotlib代码并不是学习机器学习编程的重要部分。

**定义绘图函数如下：**
```python
#@title 定义绘图函数
def plot_the_model(trained_weight, trained_bias, feature, label):
  """将经过训练的模型与200个随机训练示例绘制在一起。"""

  # 标记坐标轴。
  plt.xlabel(feature)
  plt.ylabel(label)

  # 从数据集中随机选择200个点创建散点图。
  random_examples = training_df.sample(n=200)
  plt.scatter(random_examples[feature], random_examples[label])

  # 创建一个红色线条表示模型。红色线条起点坐标为(x0, y0)，终点坐标为(x1, y1)。
  x0 = 0
  y0 = trained_bias
  x1 = random_examples[feature].max()
  y1 = trained_bias + (trained_weight * x1)
  plt.plot([x0, x1], [y0, y1], c='r')

  # 绘制散点图和红色线条。
  plt.show()


def plot_the_loss_curve(epochs, rmse):
  """绘制损失与epoch的曲线。"""

  plt.figure()
  plt.xlabel("Epoch")
  plt.ylabel("Root Mean Squared Error")

  plt.plot(epochs, rmse, label="Loss")
  plt.legend()
  plt.ylim([rmse.min()*0.97, rmse.max()])
  plt.show()  

print("已定义plot_the_model和plot_the_loss_curve函数。")
```
**调用模型函数**

机器学习的一个重要部分是确定哪些特征与标签相关。

例如，真实生活中的房屋价值预测模型通常依赖于数百个特征和合成特征。然而，这个模型只依赖于一个特征。

目前，您将任意选择total_rooms作为该特征。

```python
# 以下变量是超参数。
learning_rate = 0.01
epochs = 30
batch_size = 30

# 指定特征和标签。
my_feature = "total_rooms"  # 特定城市街区的总房间数。
my_label = "median_house_value"  # 特定城市街区的房屋中位数价值。
# 也就是说，您将创建一个仅基于total_rooms预测房屋价值的模型。

# 丢弃任何已存在的模型版本。
my_model = None

# 调用函数。
my_model = build_model(learning_rate)
weight, bias, epochs, rmse = train_model(my_model, training_df,
                                         my_feature, my_label,
                                         epochs, batch_size)

print("\nThe learned weight for your model is %.4f" % weight)
print("The learned bias for your model is %.4f\n" % bias )

plot_the_model(weight, bias, my_feature, my_label)
plot_the_loss_curve(epochs, rmse)
```
```shell
Epoch 29/30
567/567 [==============================] - 1s 1ms/step - loss: 15469.9658 - root_mean_squared_error: 124.3783
Epoch 30/30
567/567 [==============================] - 1s 1ms/step - loss: 15328.4785 - root_mean_squared_error: 123.8082

The learned weight for your model is 0.0215
The learned bias for your model is 132.1367

/usr/local/lib/python3.10/dist-packages/numpy/core/shape_base.py:65: VisibleDeprecationWarning: Creating an ndarray from ragged nested sequences (which is a list-or-tuple of lists-or-tuples-or ndarrays with different lengths or shapes) is deprecated. If you meant to do this, you must specify 'dtype=object' when creating the ndarray.
  ary = asanyarray(ary)
```
![图](https://jeasyplus.com/images/machine-learning/2023-06-27-15.49.01.png)
![图](https://jeasyplus.com/images/machine-learning/2023-06-27-15.49.02.png)

在训练模型时，会有一定的随机性影响。因此，每次训练模型时都会得到不同的结果。

也就是说，鉴于数据集和超参数，训练后的模型通常无法很好地描述特征与标签之间的关系。

**使用模型进行预测**

您可以使用训练好的模型进行预测。在实际应用中，您应该对未在训练中使用的示例进行预测。但是，在此练习中，您将只使用相同训练数据集的子集进行操作。稍后的 Colab 练习将探讨如何对未在训练中使用的示例进行预测。

首先，运行以下代码来定义房屋预测函数：
```python
def predict_house_values(n, feature, label):
  """根据特征预测房屋价值。"""

  batch = training_df[feature][10000:10000 + n]
  predicted_values = my_model.predict_on_batch(x=batch)

  print("feature   label          predicted")
  print("  value   value          value")
  print("          in thousand$   in thousand$")
  print("--------------------------------------")
  for i in range(n):
    print ("%5.0f %6.0f %15.0f" % (training_df[feature][10000 + i],
                                   training_df[label][10000 + i],
                                   predicted_values[i][0] ))
```
现在，对10个示例调用房屋预测函数：
```python
  predict_house_values(10, my_feature, my_label)
```
```shell
feature   label          predicted
  value   value          value
          in thousand$   in thousand$
--------------------------------------
 1960     53             174
 3400     92             205
 3677     69             211
 2202     62             179
 2403     80             184
 5652    295             254
 3318    500             203
 2552    342             187
 1364    118             161
 3468    128             207
```

**任务2：评估模型的预测能力**

观察前面的表格。预测值与标签值有多接近？换句话说，您的模型是否准确预测房屋价值？

```shell
# 大多数预测值与标签值相差较大，所以训练的模型可能没有太多的预测能力。
# 然而，前10个示例可能不代表其他示例的情况。
```

**任务3：尝试不同的特征**

总房间数特征的预测能力很低。使用人口数量作为特征是否能够提供更好的预测能力？尝试将特征更改为人口数量，而不是总房间数。

注意：当更改特征时，您可能还需要更改超参数。

```python
my_feature = "?"   # 将 "?" 替换为 "population" 或其他列名。

# 调整超参数进行实验。
learning_rate = 2
epochs = 3
batch_size = 120

# 不要更改此行以下的任何内容。
my_model = build_model(learning_rate)
weight, bias, epochs, rmse = train_model(my_model, training_df, 
                                         my_feature, my_label,
                                         epochs, batch_size)
plot_the_model(weight, bias, my_feature, my_label)
plot_the_loss_curve(epochs, rmse)

predict_house_values(15, my_feature, my_label)
```
实际代码
```python
my_feature = "population" # Pick a feature other than "total_rooms"

# Possibly, experiment with the hyperparameters.
learning_rate = 0.05
epochs = 18
batch_size = 3

# Don't change anything below.
my_model = build_model(learning_rate)
weight, bias, epochs, rmse = train_model(my_model, training_df, 
                                         my_feature, my_label,
                                         epochs, batch_size)

plot_the_model(weight, bias, my_feature, my_label)
plot_the_loss_curve(epochs, rmse)

predict_house_values(10, my_feature, my_label)
```
```shell
5667/5667 [==============================] - 8s 1ms/step - loss: 18132.3945 - root_mean_squared_error: 134.6566
Epoch 18/18
5667/5667 [==============================] - 8s 1ms/step - loss: 17837.1973 - root_mean_squared_error: 133.5560
/usr/local/lib/python3.10/dist-packages/numpy/core/shape_base.py:65: VisibleDeprecationWarning: Creating an ndarray from ragged nested sequences (which is a list-or-tuple of lists-or-tuples-or ndarrays with different lengths or shapes) is deprecated. If you meant to do this, you must specify 'dtype=object' when creating the ndarray.
  ary = asanyarray(ary)
```
![图](https://jeasyplus.com/images/machine-learning/2023-06-27-15.49.03.png)
![图](https://jeasyplus.com/images/machine-learning/2023-06-27-15.49.05.png)
```shell
feature   label          predicted
  value   value          value
          in thousand$   in thousand$
--------------------------------------
 1286     53             153
 1867     92             127
 2191     69             113
 1052     62             163
 1647     80             137
 2312    295             108
 1604    500             139
 1066    342             163
  338    118             195
 1604    128             139
```
**人口数量(population)**是否产生了比**总房间数(total_rooms)** 更好的预测结果？
```shell
# 训练并不完全确定性，但通常情况下，人口数量的均方根误差略高于总房间数。
# 因此，人口数量在进行预测方面与总房间数大致相同或稍差一些。
```

**任务 4：定义一个合成特征**

你已经确定了总房间数和人口数量这两个特征并不有用。

也就是说，无论是社区的总房间数还是社区的人口数量都没有成功预测该社区的房屋中位数价格。

然而，也许总房间数与人口数量的比值可能具有一定的预测能力。也就是说，区块密度可能与房屋中位数价格相关。

为了探索这个假设，进行以下操作：

+ 创建一个合成特征，它是总房间数与人口数量的比值。（如果你对pandas DataFrame不熟悉，请学习Pandas DataFrame超快速教程。）


+ 调整三个超参数。


+ 确定这个合成特征是否比你之前尝试过的任何单个特征产生更低的损失值。

```python
# 定义一个名为 rooms_per_person 的合成特征
training_df["rooms_per_person"] = ?  # 在这里编写你的代码。

# 不要改变下一行。
my_feature = "rooms_per_person"

# 为这三个超参数赋值。
learning_rate = ?
epochs = ?
batch_size = ?

# 不要改变下面的任何内容。
my_model = build_model(learning_rate)
weight, bias, epochs, rmse = train_model(my_model, training_df,
                                         my_feature, my_label,
                                         epochs, batch_size)

plot_the_loss_curve(epochs, rmse)
predict_house_values(15, my_feature, my_label)
```
实际代码：
```python
#@title Double-click to view a possible solution to Task 4.

# Define a synthetic feature
training_df["rooms_per_person"] = training_df["total_rooms"] / training_df["population"]
my_feature = "rooms_per_person"

# Tune the hyperparameters.
learning_rate = 0.06
epochs = 24
batch_size = 30

# Don't change anything below this line.
my_model = build_model(learning_rate)
weight, bias, epochs, mae = train_model(my_model, training_df,
                                        my_feature, my_label,
                                        epochs, batch_size)

plot_the_loss_curve(epochs, mae)
predict_house_values(15, my_feature, my_label)
```
```shell
Epoch 23/24
567/567 [==============================] - 1s 2ms/step - loss: 13381.8252 - root_mean_squared_error: 115.6798
Epoch 24/24
567/567 [==============================] - 1s 2ms/step - loss: 13357.2529 - root_mean_squared_error: 115.5736
```
![图](https://jeasyplus.com/images/machine-learning/2023-06-27-15.49.07.png)
```shell
feature   label          predicted
  value   value          value
          in thousand$   in thousand$
--------------------------------------
    2     53             190
    2     92             201
    2     69             196
    2     62             212
    1     80             187
    2    295             226
    2    500             211
    2    342             224
    4    118             289
    2    128             215
    2    187             225
    3     80             235
    2    112             226
    2     95             220
    2     69             211
```

根据损失值，这个合成特征产生的模型比你在任务2和任务3中尝试的单个特征模型更好。

然而，这个模型仍然不能产生很好的预测结果。

**任务 5. 寻找与标签相关的特征**

到目前为止，我们依靠试错的方法来确定可能用于模型的特征。现在，我们将依靠统计数据来进行分析。

相关矩阵显示每个属性的原始值与其他属性的原始值之间的关系。相关系数具有以下含义：

+ 1.0：完全正相关；即一个属性上升，另一个属性也上升。

+ -1.0：完全负相关；即一个属性上升，另一个属性下降。

+ 0.0：无相关性；两列之间没有线性关系。

通常情况下，相关系数的绝对值越大，其预测能力就越强。例如，相关系数为-0.8的预测能力远远优于相关系数为-0.2的预测能力。

以下代码单元格生成了加利福尼亚住房数据集属性的相关矩阵：
```python
# Generate a correlation matrix.
training_df.corr()
```
```python
correlation_matrix = """
|                           | longitude | latitude | housing_median_age | total_rooms | total_bedrooms | population | households | median_income | median_house_value | rooms_per_person |
|---------------------------|-----------|----------|--------------------|-------------|----------------|------------|------------|---------------|--------------------|-----------------|
| longitude                 | 1.0       | -0.9     | -0.1               | 0.0         | 0.1            | 0.1        | 0.1        | -0.0          | -0.0               | -0.1            |
| latitude                  | -0.9      | 1.0      | 0.0                | -0.0        | -0.1           | -0.1       | -0.1       | -0.1          | 0.1                | 0.1             |
| housing_median_age        | -0.1      | 0.0      | 1.0                | -0.4        | -0.3           | -0.3       | -0.3       | -0.1          | 0.1                | -0.1            |
| total_rooms               | 0.0       | -0.0     | -0.4               | 1.0         | 0.9            | 0.9        | 0.9        | 0.2           | 0.1                | 0.1             |
| total_bedrooms            | 0.1       | -0.1     | -0.3               | 0.9         | 1.0            | 0.9        | 1.0        | -0.0          | 0.0                | 0.0             |
| population                | 0.1       | -0.1     | -0.3               | 0.9         | 0.9            | 1.0        | 0.9        | -0.0          | -0.0               | -0.1            |
| households                | 0.1       | -0.1     | -0.3               | 0.9         | 1.0            | 0.9        | 1.0        | 0.0           | 0.1                | -0.0            |
| median_income             | -0.0      | -0.1     | -0.1               | 0.2         | -0.0           | -0.0       | 0.0        | 1.0           | 0.7                | 0.2             |
| median_house_value        | -0.0      | -0.1     | 0.1                | 0.1         | 0.0            | -0.0       | 0.1        | 0.7           | 1.0                | 0.2             |
| rooms_per_person          | -0.1      | 0.1      | -0.1               | 0.1         | 0.0            | -0.1       | -0.0       | 0.2           | 0.2                | 1.0             |
"""
print(correlation_matrix)
```

相关矩阵显示了九个潜在特征（包括一个合成特征）和一个标签（median_house_value）。

与标签具有强烈的负相关或正相关关系的特征可能是一个很好的候选特征。

**你的任务是：** 确定这九个潜在特征中哪一个似乎是最佳的特征候选者？
```shell
# median_income与标签median_house_value的相关系数为0.7，

# 所以median_income可能是一个很好的特征候选者。

# 其他七个潜在特征的相关系数都相对接近于0。

# 如果时间允许，尝试将median_income作为特征

# 看看模型是否有所改善。
```
相关矩阵并不能完全说明问题。在后续的练习中，你将发现其他方法来挖掘潜在特征的预测能力。

**注意：** 使用median_income作为特征可能会引起一些道德和公平性问题。在课程的最后阶段，我们将探讨道德和公平性问题。