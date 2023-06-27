# 泛化

**泛化**和**拟合**是机器学习中相对的概念。

**拟合**是指模型对已知数据的拟合程度，而**泛化**是指模型在未见过的数据上的表现能力。

我们希望通过适当的**拟合**来达到良好的**泛化**，即在训练数据和新数据上都能取得较好的性能。

**过度拟合的模型**

![图](https://jeasyplus.com/images/machine-learning/2023-06-27-17.32.59.png)

**过度拟合的问题**
![图](https://jeasyplus.com/images/machine-learning/2023-06-27-17.31.37.png)


**泛化相关概念**

![图](https://jeasyplus.com/images/machine-learning/2023-06-27-17.34.57.png)
![图](https://jeasyplus.com/images/machine-learning/2023-06-27-17.35.24.png)
![图](https://jeasyplus.com/images/machine-learning/2023-06-27-17.35.51.png)

## 泛化：过拟合的风险

本模块关注概括化。为了对这个概念有一些直觉理解，你将看到三个图形。假设这些图形中的每个点代表森林中一棵树的位置。

两种颜色的含义如下：

+ 蓝色点代表生病的树。


+ 橙色点代表健康的树。

在这个前提下，我们来看一下图1。

![图](https://jeasyplus.com/images/machine-learning/2023-06-27-17.35.53.png)

图1：生病（蓝色）和健康（橙色）的树。

你能想象出一个能够预测接下来是生病树还是健康树的好模型吗？花一点时间在脑海中画一个将蓝色和橙色区分开的弧线，或者在脑海中圈选一批橙色或蓝色的树。接下来，看一下图2，展示了一个特定的机器学习模型如何将生病的树和健康的树分开。请注意，该模型产生了非常低的损失值。

**图2：一个用于区分生病树和健康树的复杂模型。**


![图](https://jeasyplus.com/images/machine-learning/2023-06-27-17.35.55.png)

乍一看，图2中展示的模型似乎非常成功地将健康树和生病树分开了。但是，真的是这样吗？


**低损失，但仍然是一个糟糕的模型吗？**


图3展示了当我们向模型添加新数据时发生的情况。结果发现，该模型对新数据的适应性非常差。请注意，该模型错误分类了大部分新数据。


![图](https://jeasyplus.com/images/machine-learning/2023-06-27-17.35.57.png)


**图3：该模型在预测新数据时表现不佳。**

图2和图3中展示的模型过度拟合了训练数据的特征。过拟合的模型在训练过程中获得了低损失，但在预测新数据时表现不佳。如果一个模型在当前样本上拟合得很好，我们如何相信它在新数据上会做出良好的预测？正如之后将会看到的，过拟合是由于使模型比必要复杂所导致的。机器学习的基本矛盾在于既要很好地拟合数据，又要尽可能简化数据的拟合。

机器学习的目标是在来自（隐藏的）真实概率分布的新数据上进行良好的预测。不幸的是，模型无法看到整个真相；模型只能从训练数据集中进行采样。如果一个模型在当前样本上拟合得很好，你如何相信这个模型也会对之前从未见过的样本做出良好的预测？

14世纪的修士和哲学家奥卡姆的威廉（William of Ockham）崇尚简洁。他认为科学家应该更倾向于简单的公式或理论，而不是更复杂的公式或理论。用机器学习的术语来说，奥卡姆的剃刀原则是：

```python
机器学习模型越简单，良好的实证结果越有可能不仅仅是由于样本的特殊性所致。
```

在现代，我们将奥卡姆的剃刀原则形式化为统计学习理论和计算学习理论的领域。这些领域发展出了概括界限（generalization bounds），它们是基于以下因素对模型泛化能力的统计描述：

+ 模型的复杂性


+ 模型在训练数据上的性能

虽然理论分析在理想化的假设下提供了正式的保证，但在实践中往往难以应用。《机器学习速成课程》相反，专注于经验评估来判断模型对新数据的泛化能力。

机器学习模型的目标是对新的、以前未见过的数据进行良好的预测。但是，如果你正在使用自己的数据集构建模型，你如何获取以前未见过的数据呢？ 一种方法是将数据集分成两个子集：

+ 训练集：用于训练模型的子集。


+ 测试集：用于测试模型的子集。


测试集上的良好表现通常是对新数据整体上良好表现的有用指标，前提是：

+ 测试集足够大。


+ 你没有通过反复使用同一测试集来作弊。

## 机器学习的细节

以下三个基本假设指导着泛化：

+ 我们从分布中以独立同分布（i.i.d）的方式随机抽取样本。换句话说，样本之间不会相互影响。（另一种解释：i.i.d是指随机变量的随机性）


+ 分布是平稳的；也就是说，在数据集内部分布不会改变。


+ 我们从相同分布的分区中抽取样本。


在实践中，我们有时会违反这些假设。例如：

+ 考虑一个选择广告展示的模型。如果模型在选择广告时部分基于用户之前看过的广告，那么会违反独立同分布的假设。


+ 考虑一个包含一年零售销售信息的数据集。用户的购买会随季节性变化，这会违反平稳性。


当我们知道前述三个基本假设中的任何一个被违反时，我们必须对度量指标进行仔细的注意。
