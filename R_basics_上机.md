# R Basics 上机任务
## （1）
iris共5列。数据类型分别为
1. Numeric
2. Numeric
3. Numeric
4. Numeric
5. Factor

## (2)
### 代码
```
# 计算均值和标准差
mean_data <- aggregate(Sepal.Length ~ Species, data = iris, FUN = mean)
sd_data <- aggregate(Sepal.Length ~ Species, data = iris, FUN = sd)

# 合并结果并保存为CSV
result <- data.frame(
  Species = mean_data$Species,
  Mean = round(mean_data$Sepal.Length, 2),
  SD = round(sd_data$Sepal.Length, 2)
)
write.csv(result, "iris_stats.csv", row.names = FALSE)
```
### 结果
```
"Species","Mean","SD"
"setosa",5.01,0.35
"versicolor",5.94,0.52
"virginica",6.59,0.64
```

## (3)
### 代码
```
# 执行ANOVA分析
aov_model <- aov(Sepal.Width ~ Species, data = iris)
summary(aov_model)
```
### 结果
```
             Df Sum Sq Mean Sq F value Pr(>F)    
Species       2  11.35   5.672   49.16 <2e-16 ***
Residuals   147  16.96   0.115                   
---
Signif. codes:  
0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
```
