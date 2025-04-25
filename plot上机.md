脚本：<br>
```
library(ggplot2)

ggplot(iris, aes(x = Species, y = Sepal.Length)) +
  geom_violin(aes(fill = Species), trim = FALSE) +
  labs(title = "Sepal Length Distribution") +
  theme(plot.title = element_text(hjust = 0.5, face = "bold")) +
  scale_y_continuous(limits = c(0.5, 7)) + ## 可是会截断大于7的数据
  scale_fill_manual(values = c("#C44E52", "#55A868", "#4C72B0"))
```
结果：
![ggplot上机](https://github.com/user-attachments/assets/53f1e3fe-8100-4bb1-9377-2f1b1b958439)
