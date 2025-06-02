# Homework 10
## R
### (1)
**代码**

```
install.packages("glmnet")
install.packages("caret")
install.packages("pROC")
install.packages("mlbench")
install.packages("randomForest")

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

# 可视化（使用ggplot2）
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
# 80% data for training, 20% for performance evaluation
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

![Rplot](https://github.com/user-attachments/assets/d421275d-9962-4b9e-a04b-f58d97f2c6ae)
![Rplot PCA](https://github.com/user-attachments/assets/fb8e63b2-affb-4b46-9c10-7cd96f20abef)
