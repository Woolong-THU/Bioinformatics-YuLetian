# Homework 8
## GO
### (1)
![HeatmapSelectedGO](https://github.com/user-attachments/assets/2cee0d13-8869-43b3-a8a0-71a248121404)

### (2)
**Fold Enrichment（FE）的计算公式及原理**  
Fold Enrichment衡量某个GO term在差异表达基因中的富集程度，计算公式为：  
$\text{FE} = \frac{(k/n)}{(M/N)} = \frac{k \cdot N}{n \cdot M}$ 

其中：  
- \(k\)：差异基因中属于该GO term的数目  
- \(n\)：差异基因总数  
- \(M\)：全基因组中属于该GO term的基因数目  
- \(N\)：全基因组总基因数  

**原理**：FE表示差异基因中该GO term的比例与基因组背景比例的比值。若FE > 1，表明该GO term在差异基因中富集。

---

**P值的计算及原理**  
P值通过**超几何检验**（或费舍尔精确检验）计算，检验差异基因中某GO term的基因数目是否显著高于随机期望。公式为：  
$P = \sum_{i=k}^{\min(n, M)} \frac{\binom{M}{i} \binom{N-M}{n-i}}{\binom{N}{n}}$

**原理**：计算在随机情况下，从基因组中抽取\(n\)个基因时，至少包含\(k\)个该GO term基因的概率。若P值很小，表明富集程度具有统计学显著性。

---

**为何使用FDR而非P值定义显著富集**  
1. **多重检验问题**：GO分析涉及成百上千个GO terms，直接使用P值（如阈值0.05）会导致大量假阳性（例如，1000次检验中平均50个假阳性）。  
2. **FDR控制错误发现率**：FDR（False Discovery Rate，如Benjamini-Hochberg校正）控制的是**显著结果中假阳性的比例**，而非单次检验的错误率，更适合大规模比较。  
3. **提高结果可靠性**：相较于严格控制Family-Wise Error Rate（FWER）的Bonferroni校正，FDR在保持统计效力的同时更灵活，适用于生物数据分析。

**总结**：FDR通过校正多重比较的影响，平衡假阳性与统计效力，是定义显著富集的更优指标。

## KEGG
![HeatmapSelectedGO](https://github.com/user-attachments/assets/f2abb7a9-00c1-4eb5-9640-5fd48999ae95)
