layout: default
title: Lecture1 笔记

# 生物信息学 Lecture 1
## 评分标准
- 20%课堂互动
- 80%当堂/课后作业
- 10%Pre一次
- 加分题期末

## Introduction-Part 1
Question->Information->Analysis->Modeling
### Information
Images or Sequences
#### Seq来自一代、二代（NGS）、三代……测序
Type of NGS
1. DNA -seq
2. RNA -seq
3. Epigenetics
  - DNAase
  - Methylation
  - Histone modifications ChIP-seq
4. Interaction
  - pro-DNA:ChIP-seq
  - pro-RNA:CLIP-seq
  - DNA-RNA:Grid-seq

#### Metagenomics宏基因组学
- 从环境中提取样本
- 从组织中提取

### Analysis
#### NGS Data Analysis
- Interpreting the  Data

### Modeling
区分：
- Probabilistic model
- Computational algorithm
#### model
- regression model
- neural network model
- 从语言学迁移模型
#### algorithm
- Number sorting algorithm
- Dynamic programming algorithm
- ……

## 作业
getting started 和 setup里一共两个


### 概率模型（Probabilistic Model）
**定义**：
- 概率模型是一种用于描述随机现象的数学模型。它使用概率理论来量化不确定性，并预测不同结果的可能性。<br>
**特点**：
- **随机性**：考虑事件发生的不确定性。
- **概率分布**：使用概率分布（如正态分布、二项分布等）来描述数据。
- **参数化**：模型通常包含参数，这些参数描述了数据的特征。<br>
**应用**：
- **统计学**：用于假设检验、置信区间等。
- **机器学习**：如朴素贝叶斯分类器、隐马尔可夫模型等。
- **金融**：用于风险评估、期权定价等。<br>
**例子**：
- 抛硬币模型：描述硬币正面和反面出现的概率。
- 随机漫步模型：描述股票价格的变化。
### 计算算法（Computational Algorithm）
**定义**：
- 计算算法是一系列明确的指令，用于解决特定问题或执行特定任务。它通常在计算机上实现，但也可以手动执行。<br>
**特点**：
- **确定性**：算法的每一步都是确定的，没有随机性。
- **效率**：关注算法的执行时间（时间复杂度）和内存使用（空间复杂度）。
- **可重复性**：相同的输入总是产生相同的输出。<br>
**应用**：
- **计算机科学**：如排序算法（快速排序、归并排序等）、搜索算法（二分搜索等）。
- **数据科学**：如机器学习算法（梯度下降、随机森林等）。
- **日常任务**：如烹饪食谱、组装指南等。<br>
**例子**：
- 快速排序算法：一种高效的排序方法。
- 梯度下降算法：用于优化和机器学习中的参数估计。
### 区别
- **目的**：概率模型旨在描述和预测随机现象，而计算算法旨在解决问题或执行任务。
- **性质**：概率模型包含随机性，而计算算法通常是确定性的。
- **结构**：概率模型由概率分布和参数组成，而计算算法由一系列指令组成。
- **应用领域**：概率模型更广泛应用于统计学和不确定性分析，而计算算法更广泛应用于计算机科学和工程问题解决。
尽管两者有所区别，但在实际应用中，它们经常结合使用。例如，在机器学习中，概率模型可以用于描述数据生成过程，而计算算法可以用于优化模型参数。

