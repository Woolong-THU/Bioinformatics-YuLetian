# Homework 8
## GO
### (1)
![HeatmapSelectedGO](https://github.com/user-attachments/assets/2cee0d13-8869-43b3-a8a0-71a248121404)

### (2)
#### **Fold Enrichment（FE）**  
Fold Enrichment衡量某个GO term在差异表达基因中的富集程度，计算公式为：  
$\text{FE} = \frac{(k/n)}{(M/N)} = \frac{k \cdot N}{n \cdot M}$ 

其中：  
- k：差异基因中属于该GO term的数目  
- n：差异基因总数  
- M：全基因组中属于该GO term的基因数目  
- N：全基因组总基因数  

**原理**：FE表示差异基因中该GO term的比例与基因组背景比例的比值。若FE > 1，表明该GO term在差异基因中富集。

---

#### **P值**  
P值通过超几何检验计算，检验差异基因中某GO term的基因数目是否显著高于随机期望。公式为：  
$P = \sum_{i=k}^{\min(n, M)} \frac{\binom{M}{i} \binom{N-M}{n-i}}{\binom{N}{n}}$

**原理**：计算在随机情况下，从基因组中抽取\(n\)个基因时，至少包含\(k\)个该GO term基因的概率。若P值很小，表明富集程度具有统计学显著性。

---

**为何使用FDR而非P值定义显著富集**  
1. **多重检验问题**：GO分析涉及成百上千个GO terms，直接使用P值（如阈值0.05）会导致大量假阳性。  
2. **FDR控制错误发现率**：FDR控制的是显著结果中假阳性的比例，而非单次检验的错误率，更适合大规模比较。  

## KEGG
![HeatmapSelectedGO](https://github.com/user-attachments/assets/f2abb7a9-00c1-4eb5-9640-5fd48999ae95)

### 相同点
1. GO和KEGG都表明光照（尤其是蓝光、UV-A）显著激活光信号通路，调控基因表达。两者均涉及昼夜节律​，说明生物钟基因整合光信号以协调生理活动的周期性变化。<br>
2. 都显示与类黄酮代谢、谷胱甘肽代谢相关。<br>

### 不同点
1.
  - GO强调基因功能的动态调控过程。覆盖更广泛的生物学场景，如胁迫响应、激素信号。
  - KEGG聚焦代谢通路的具体步骤，明确基因在通路中的具体作用。

2.
  - GO分析广度优先，涵盖多维度功能​（如光响应、次生代谢、胁迫适应），但缺乏通路分子互作细节。
  - ​KEGG分析深度优先，但未覆盖非代谢相关过程。
