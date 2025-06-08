# Homework 10
## (1)
**代码**

```
library(glmnet)
library(caret)
library(pROC)
library(mlbench)
library(randomForest)

# 用均值补全缺失值，用Z-score对特征进行scaling
qPCR_data = read.csv("~/Desktop/qPCR_data.csv") 
y <- factor(qPCR_data[,13], levels = c("NC", "HCC"))
x <- qPCR_data[,2:12]
x <- apply(x,2,as.numeric)
feature.mean <- colMeans(x,na.rm = T)
# fill missing value with average value of this feature 
x[is.na(x)] <- matrix(rep(feature.mean,each=length(y)), nrow=length(y))[is.na(x)]
# Z score scaling
x <- scale(x,center = TRUE,scale = TRUE)

# PCA分析（使用预处理后的全数据集）
pca_result <- prcomp(x, scale. = FALSE)  # 已标准化无需再缩放

# 创建可视化数据框
pca_df <- data.frame(
  PC1 = pca_result$x[, 1],
  PC2 = pca_result$x[, 2],
  Label = y
)

# 计算方差解释率
variance_percent <- round(100 * pca_result$sdev^2 / sum(pca_result$sdev^2), 2)

# PCA可视化（使用ggplot2）
library(ggplot2)
ggplot(pca_df, aes(x = PC1, y = PC2, color = Label)) +
  geom_point(size = 3, alpha = 0.7) +                
  stat_ellipse(level = 0.9, linewidth = 0.8) +       
  scale_color_manual(values = c("NC" = "#377eb8",     
                                "HCC" = "#e41a1c")) +
  labs(title = "PCA Visualization",
       x = paste("PC1 (", variance_percent[1], "%)"), 
       y = paste("PC2 (", variance_percent[2], "%)")) +
  theme_minimal(base_size = 12) +                    
  theme(
    legend.position = "right",
    panel.grid.major = element_line(color = "grey90"),
    panel.border = element_rect(fill = NA, color = "grey70")
  )

# 数据集划分(分层抽样)
set.seed(666) # 固定random seed保证结果可重复
# 80% 数据用于训练模型，20% 用于检验
train.indices <- createDataPartition(y,p=0.8,times = 1,list=T)$Resample1
x.train <- x[train.indices,]
x.test <- x[ -train.indices,]
y.train <- y[train.indices]
y.test <- y[ -train.indices]
y.train <- factor(as.character(y.train), levels = c("NC", "HCC"))

# 特征选择（RFE）
rfFuncs$summary <- twoClassSummary
rfeCtrl <- rfeControl(functions = rfFuncs,
                      method = "cv",
                      number = 10,
                      verbose = FALSE)

rfe.results <- rfe(x.train, y.train,
                   sizes = 2:9, 
                   rfeControl = rfeCtrl,
                   metric = "ROC")
selected.features <- predictors(rfe.results)
x.train.selected <- x.train[, selected.features, drop = FALSE]
x.test.selected <- x.test[, selected.features, drop = FALSE]

# 模型训练与调参（使用glmnet）
trainCtrl <- trainControl(method = "cv",
                          number = 10,
                          classProbs = TRUE,
                          summaryFunction = twoClassSummary)

glmnetGrid <- expand.grid(alpha = seq(0, 1, length = 5),
                          lambda = 10^seq(-3, 0, length = 20))

set.seed(666)
glmnetModel <- train(x.train.selected, y.train,
                     method = "glmnet",
                     trControl = trainCtrl,
                     tuneGrid = glmnetGrid,
                     metric = "ROC")

# 模型评估
testProbs <- predict(glmnetModel, x.test.selected, type = "prob")
rocObj <- roc(y.test, testProbs$HCC, 
              levels = c("NC", "HCC"))  
aucValue <- auc(rocObj)

# 可视化结果
par(pty = "s")  # 设置图形为正方形
plot(rocObj, 
     main = paste0("ROC Curve (AUC = ", round(aucValue, 3), ")"),
     legacy.axes = TRUE,
     print.auc = TRUE)
grid()
```

**PCA结果**
![PCA结果](https://github.com/user-attachments/assets/cf1e1815-3520-4c79-8474-ac9d9d02eaa0)

**ROC曲线**
![ROC曲线](https://github.com/user-attachments/assets/de7ac983-5ac2-43b2-9df7-bf47372fef27)

AUC=0.88 模型区分能力良好。<br>

## (2)
### Q1
不是。​​树的数量通常不需要通过交叉验证进行精细调整以达到最优值.<br>
**原因**<br>
- 边际效应递减：随着树的数量增加，模型的预测能力会提升。但是，这种提升遵循边际效应递减规律。增加最初的树效果会非常显著，但增加到很多后，提升幅度会变得越来越小，逐渐趋于平缓。
- 过多不会导致过拟合：增加更多的树并不会使模型更容易过拟合基础训练数据。它主要是让模型的预测变得更加稳定和可靠。相比之下，让单棵树变得更深则可能导致单棵树过拟合。<br>
### Q2
- 对于原始训练集中的每一个样本，我们可以找出所有那些在构建过程中没有使用过该样本的树。用这些树的预测结果进行聚合，得到该样本的一个预测值。这被称为OOB预测。OOB Error 就是使用这些 OOB 预测对整个训练集计算的预测误差。
- OOB Error的存在完全依赖于随机森林构建过程对Bootstrapping方法的应用。正是因为对每一棵树使用了有放回抽样，才天然地产生了一部分未被选中的样本（OOB样本）。OOB Error是利用Bootstrapping过程产生的OOB样本作为天然验证集，来计算模型性能的一种方法。
