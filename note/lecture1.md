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


## 模型与算法的区别可以从以下几个维度进行系统阐述
### 核心定义差异

1. **模型（Model）**  
   - 是对现实世界问题或系统的数学/逻辑抽象表示  
   - 本质：静态的知识表达结构  
   - 示例：  
     - 概率模型（描述随机变量间的关系）  
     - 回归模型
     - 神经网络模型（层结构+激活函数）

2. **算法（Algorithm）**  
   - 是解决问题的明确步骤序列  
   - 本质：动态的计算过程  
   - 示例：  
     - 梯度下降算法（参数优化过程）  
     - 快速排序算法（元素比较交换步骤）  
     - 动态规划（状态转移方程应用流程）

---

### 角色定位对比

| 维度        | 模型                          | 算法                          |
|-------------|-----------------------------|-----------------------------|
| **功能**    | 知识容器（存储规律/模式）         | 操作引擎（执行计算/推理）         |
| **输入**    | 参数（如权重、系数）              | 初始数据+控制参数              |
| **输出**    | 预测/分类结果                  | 问题解决方案                  |
| **可变性**  | 通过训练改变内部参数             | 固定步骤流程（可调参数）        |

---

### 依赖关系

1. **模型依赖算法**  
   - 训练阶段：需要优化算法（如SGD）确定模型参数  
   - 推理阶段：需要预测算法（如矩阵乘法）计算结果

2. **算法独立于模型**  
   - 同一算法可服务不同模型（如梯度下降可用于线性回归和神经网络）  
   - 同一模型可用不同算法实现（如线性回归可用最小二乘法或梯度下降）

---

### 抽象层级

1. **模型是高层抽象**  
   - 关注"what"（描述系统本质）  
   - 如：语言迁移模型描述知识跨语言传递的机制

2. **算法是底层实现**  
   - 关注"how"（具体执行方法）  
   - 如：Transformer中的自注意力计算步骤
  
---

### 总结

模型与算法的关系如同"汽车设计图与装配流水线"：  
- 模型是系统的知识蓝图，决定能力边界  
- 算法是实现的工具链，决定执行效率  

