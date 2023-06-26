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
"""
To try the examples in the browser:
1. Type code in the input cell and press
   Shift + Enter to execute
2. Or copy paste the code, and click on
   the "Run" button in the toolbar
"""

# The standard way to import NumPy:
import numpy as np

# Create a 2-D array, set every second element in
# some rows and find max per row:

x = np.arange(15, dtype=np.int64).reshape(3, 5)
x[1:, ::2] = -99
x
# array([[  0,   1,   2,   3,   4],
#        [-99,   6, -99,   8, -99],
#        [-99,  11, -99,  13, -99]])

x.max(axis=1)
# array([ 4,  8, 13])

# Generate normally distributed random numbers:
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
综上所述，NumPy和Pandas是两个重要的Python库，NumPy主要用于数值计算和数组操作，而Pandas则专注于数据处理和分析。它们在数据科学和机器学习领域扮演着重要角色，并经常与其他库（如Matplotlib和Scikit-learn）一起使用，构建完整的数据分析和机器学习工作流程。

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

print("已定义 build_model 和 train_model")
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

检查底部的图表，它显示了损失曲线。注意损失曲线的下降趋势，但没有趋于平缓，这表明模型尚未充分训练。

**任务2：增加迭代轮数**

训练损失应该逐渐降低，一开始降低得很快，然后逐渐减缓。最终，训练损失应该保持稳定（斜率为零或接近零），这表示训练已经收敛。

在**任务1**中，训练损失没有收敛。一个可能的解决方案是增加迭代轮数。你的任务是增加足够多的迭代轮数，使模型收敛。然而，过度训练也是低效的，所以不要将迭代轮数设置为任意的高值。

检查损失曲线。模型是否收敛？
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

在**任务2**中，你增加了迭代轮数以使模型收敛。
有时，通过增加学习率可以更快地使模型收敛。然而，将学习率设置得太高往往会使模型无法收敛。

在**任务3**中，我们故意将学习率设置得过高。运行下面的代码单元格，看看会发生什么。
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

红色的线与蓝色的点不对齐。此外，损失曲线像过山车一样震荡。震荡的损失曲线强烈表明学习率过高。

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

系统在每次迭代之后重新计算模型的损失值，并调整模型的权重和偏置。每次迭代是系统处理一个批次的范围。例如，如果批次大小为6，则系统在处理每6个示例之后重新计算模型的损失值，并调整模型的权重和偏置。

一个迭代轮数足够处理数据集中的每个示例。例如，如果批次大小为12，则每个迭代轮数持续一次迭代。然而，如果批次大小为6，则每个迭代轮数需要两次迭代。

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
大多数机器学习问题需要进行大量的超参数调优。不幸的是，我们无法为每个模型提供具体的调优规则。降低学习率可以帮助某个模型高效收敛，但可能会导致另一个模型收敛过慢。您必须进行实验，找到适合您数据集的最佳超参数组合。以下是一些经验法则：

+ 训练损失应该稳步下降，一开始下降得较快，然后变得更慢，直到曲线的斜率达到或接近零。
+ 如果训练损失没有收敛，增加训练轮数。
+ 如果训练损失下降得太慢，增加学习率。请注意，将学习率设置得太高也可能阻止训练损失的收敛。
+ 如果训练损失波动很大（即训练损失跳动不稳定），降低学习率。
+ 降低学习率的同时增加训练轮数或批次大小通常是一个不错的组合。
+ 将批次大小设置为非常小的值可能会导致不稳定性。首先，尝试较大的批次大小值。然后，逐渐减小批次大小，直到出现性能下降。
+ 对于包含大量示例的实际数据集，整个数据集可能无法一次性装入内存。在这种情况下，您需要减小批次大小，以使一个批次能够适应内存中。

注意：以上翻译的是原文中的一般规则和经验，实际调优过程仍需根据具体情况进行实验和探索。

## 使用合成数据进行简单线性回归
在这个第一个Colab中，你将使用一个简单的数据集来探索线性回归。

学习目标：
+ 将.csv文件读入pandas DataFrame。
+ 查看数据集。
+ 尝试使用不同的特征构建模型。
+ 调整模型的超参数。

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
这个 Colab，和许多机器学习程序一样，会将 .csv 文件读取并将数据存储在内存中，以 pandas DataFrame 的形式。Pandas 是一个开源的 Python 库，它的主要数据类型是 DataFrame。你可以将 pandas DataFrame 想象成一个电子表格，其中每一行都有一个编号，每一列都有一个名称。Pandas 本身是建立在另一个开源的 Python 库 NumPy 的基础上的。如果你对这些技术不熟悉，请查看下面这两个简短的教程：

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
对 median_house_value 进行缩放将每个房屋的值以千为单位进行表示。

缩放可以将损失值和学习率保持在更友好的范围内。

尽管通常情况下对标签进行缩放并不是必要的，但在多特征模型中进行特征缩放通常是必要的。

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