# Homework 7
# 2.1
## (1)
1. **CPM/RPM (Counts/Reads Per Million)**  
该方法通过将每个基因的读取数（reads）除以总映射读取数，再乘以百万（10^6）来进行归一化。其目的是调整数据中的测序深度差异。此方法主要用于小RNA-seq分析，适用于不考虑基因长度的情况。

2. **RPKM (Reads Per Kilobase per Million Mapped reads)**  
RPKM归一化考虑了基因的长度。其公式为：每个基因的读取数除以总的映射读取数，乘以百万，再除以该基因的长度（以千碱基为单位）。此方法同时考虑了测序深度和基因长度，适用于基因表达分析。

3. **FPKM (Fragments Per Kilobase per Million Mapped reads)**  
FPKM是RPKM的变种，用于处理成对端RNA-seq数据。FPKM = RPKM/2. 此方法适用于考虑到片段的成对端测序数据。

4. **TPM (Transcripts Per Million)**  
TPM与RPKM类似，但它首先归一化每个基因的RPKM值，然后对所有基因的RPKM值求和，最后将每个基因的RPKM值除以所有基因RPKM值的和，并乘以百万。此方法也调整了测序深度和基因长度的影响，且由于它总是归一化到百万的读取数，所以适用于不同样本之间的比较。

此外，还有其他两种专门用于差异表达分析的归一化方法：
- **TMM Counts (Trimmed Mean of M-values)**
- **RLE (Relative Log Expression)**

## (2)
E;D;A

## (3)
**代码**<br>
```
/usr/local/bin/infer_experiment.py -r GTF/Arabidopsis_thaliana.TAIR10.34.bed -i bam/Shape02.bam
```
**输出结果**<br>
```
Reading reference gene model GTF/Arabidopsis_thaliana.TAIR10.34.bed ... Done
Loading SAM/BAM file ...  Total 200000 usable reads were sampled


This is PairEnd Data
Fraction of reads failed to determine: 0.0277
Fraction of reads explained by "1++,1--,2+-,2-+": 0.4783
Fraction of reads explained by "1+-,1-+,2++,2--": 0.4939
```

"1++,1--,2+-,2-+"与"1+-,1-+,2++,2--"的比例几乎相同，故数据来源于strand nonspecific sequencing.

**AT1G09530基因(PIF3基因)上的counts数目**
```
          Geneid     counts
AT1G09530 AT1G09530   1782
```

## (4)
**脚本**
```R
# 加载包
library(edgeR)
library(pheatmap)

# 设置路径
setwd("~/Desktop/bioinfo_share_rnaseq/tumor-transcriptome-demo")

# 定义癌症类型
cancer_types <- c("COAD", "READ", "ESCA")

gene_order <- NULL
sample_data <- list()  # 使用列表存储样本数据

# 遍历每个癌症类型的样本文件
for (type in cancer_types) {
  cat("\n正在处理癌症类型:", type, "\n")
  files <- list.files(type, pattern = "\\.txt$", full.names = TRUE)
  
  for (file in files) {
    cat("正在读取文件:", file, "\n")
    
    # 使用 tryCatch 捕获并处理潜在错误
    tryCatch({
      # ------------ 读取并清理数据 ------------
      raw_lines <- readLines(file)
      cleaned_lines <- gsub('["\']', '', raw_lines)  # 移除引号
      
      data <- read.delim(
        textConnection(cleaned_lines),
        header = TRUE,
        comment.char = "#",     # 跳过注释行
        sep = "\t",             # 明确指定分隔符为制表符
        strip.white = TRUE,     # 清除字段两侧空格
        stringsAsFactors = FALSE
      )
      
      # 提取 Geneid 和最后一列（Counts）
      counts <- data.frame(
        Geneid = data$Geneid,
        Counts = data[[ncol(data)]]
      )
      
      # ------------ 处理基因顺序和样本数据 ------------
      if (is.null(gene_order)) {
        # 全局变量更新
        gene_order <<- counts$Geneid  
        
        # 初始化样本数据列表
        sample_data[[1]] <- counts$Counts
        
        # 直接对整个列表命名（避免索引越界）
        names(sample_data) <- paste(
          type, 
          tools::file_path_sans_ext(basename(file)), 
          sep = "_"
        )
        
        cat("初始化基因顺序成功！样本名称:", names(sample_data), "\n")
      } else {
        # 按基因顺序对齐当前样本
        counts_ordered <- counts[match(gene_order, counts$Geneid), ]
        sample_name <- paste(
          type, 
          tools::file_path_sans_ext(basename(file)), 
          sep = "_"
        )
        
        # 添加新样本（自动扩展列表）
        sample_data[[sample_name]] <- counts_ordered$Counts
        cat("已添加样本:", sample_name, "当前样本数:", length(sample_data), "\n")
      }
      
    }, error = function(e) {
      # 更详细的错误提示
      warning(paste(
        "文件处理失败:", file, "\n",
        "错误信息:", e$message, "\n",
        "请检查文件格式是否符合 featureCounts 输出规范"
      ))
    })
  }
}

# ------------ 转换为矩阵并后续处理 ------------
count_matrix <- do.call(cbind, sample_data)
rownames(count_matrix) <- gene_order

# 验证矩阵结构
cat("\n合并后的矩阵维度:", dim(count_matrix), "\n")  # 应为 (2000 genes) x (150 samples)

# 计算 log10 CPM
y <- DGEList(counts = count_matrix)
CPM_matrix <- cpm(y, log = FALSE)
log10_CPM_matrix <- log10(CPM_matrix + 1)

# 过滤标准差为0的基因
row_sd <- apply(log10_CPM_matrix, 1, sd)
log10_CPM_filtered <- log10_CPM_matrix[row_sd > 0, ]

# 计算Z-score矩阵
z_scores <- (log10_CPM_filtered - rowMeans(log10_CPM_filtered)) / apply(log10_CPM_filtered, 1, sd)

# 修剪Z-score范围（-2到2）
z_scores[z_scores > 2] <- 2
z_scores[z_scores < -2] <- -2

# 准备列注释（癌症类型）
annotation_col <- data.frame(
  CancerType = factor(sub("_.*", "", colnames(z_scores)))
)
rownames(annotation_col) <- colnames(z_scores)

# 自定义注释颜色（可选）
ann_colors <- list(
  CancerType = c(COAD = "#CCCCFF", READ = "#ff927c", ESCA = "#8fdeec")
)

# 绘制热图
pheatmap(
  mat = z_scores,
  cluster_rows = TRUE,
  cluster_cols = TRUE,
  show_rownames = FALSE,
  show_colnames = FALSE,
  annotation_col = annotation_col,
  annotation_colors = ann_colors,
  color = colorRampPalette(c("blue", "white", "red"))(50),
  breaks = seq(-2, 2, length.out = 50),
  main = "Z-score of log10 CPM (Grouped by Cancer Type)"
)
```
**结果**
![RNAseq_heatmap2](https://github.com/user-attachments/assets/771ad546-5117-468b-951c-6e83afa457b5)

**结论**
COAD和READ更相似。
