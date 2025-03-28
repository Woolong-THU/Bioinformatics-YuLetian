# Homework_4.1_Mapping
## (1)请阐述bowtie中利用了 BWT 的什么性质提高了运算速度？并通过哪些策略优化了对内存的需求？
Bowtie通过BWT的LF映射和反向搜索实现高效的线性时间比对，同时利用BWT的紧凑索引和分块压缩技术大幅降低内存需求。<br>
1. BWT的核心性质提升运算速度
  - BWT通过Burrows-Wheeler矩阵（BWM）的排序特性，结合LF映射（Last-First Mapping）​，使得精确匹配的时间复杂度为O(m)​（m为查询序列长度）。LF映射允许从BWT的最后一列字符快速回溯到前一列，逐步重建原始序列的上下文，无需遍历整个参考基因组。这一特性使得比对过程无需动态规划或哈希表遍历，极大提升了速度。
  - Bowtie通过反向比对（从读段的末端开始），利用BWT的BWM排序结构快速定位匹配的区间范围：每次比对一个字符时，通过维护区间[low, high]（表示在BWM中可能匹配的行范围）逐步缩小比对范围。这一过程避免了传统哈希表或动态规划中复杂的计算步骤，显著加速。
2. 内存优化策略
  - BWT的紧凑索引结构<br>
BWT将参考基因组转换为高度压缩的索引，仅需约0.5字节/碱基​（人类基因组约2GB），而哈希表或后缀数组需要超过12GB。索引仅存储BWT字符串和辅助的rank表​记录字符出现次数的累积值，而非完整的后缀数组或哈希表。<br>
  - 分块压缩与块排序<br>
BWT在构建索引时通过分块排序减少内存占用。使用小型的rank和occ表，通过位向量压缩技术进一步压缩存储。
  - ​避免存储完整后缀数组<br>
传统后缀数组需要存储所有后缀的指针（O(N log N)空间），而BWT通过LF映射动态计算后缀关系，仅需存储BWT字符串和辅助表，内存占用降低至O(N)。
## (2)
### 代码
先mapping后统计排序<br>
```
bowtie -v 2 -m 10 --best --strata BowtieIndex/YeastGenome -f THA2.fa -S THA2.sam

awk '!/^@/ && ($2 && 0x4) == 0 {chr_count[$3]++} END {for (chr in chr_count) print chr, chr_count[chr]}' THA2.sam | sort -k2,2nr
```
### 结果

<img width="97" alt="截屏2025-03-25 13 27 18" src="https://github.com/user-attachments/assets/19c8bdbe-6c10-4780-b9a6-a3fddd608b61" />

## (3)
### (3.1)
- SAM/BAM文件中的 ​CIGAR string​（Concise Idiosyncratic Gapped Alignment Report）是描述测序读段与参考基因组比对细节的核心字段，位于文件的第6列。它以紧凑的符号编码形式记录读段比对到参考基因组时的匹配、插入、缺失等操作，是解析比对结果的关键信息。
- CIGAR string 由​数字+字母的组合构成，每个字母代表一种比对操作类型，数字表示该操作连续出现的次数。其核心内容包括比对中出现的错配、插入、缺失等等序列信息。
### (3.2)
Soft clip 是指读段两端未比对到参考基因组的碱基，但仍保留在SEQ字段中。用“S”表示。”10S“表示前（后）10个碱基未比对到参考，取决于“10S”在CIGAR string开头（末尾）。
### (3.3)
- Mapping Quality（MAPQ）是SAM/BAM文件中记录的一个重要指标，用于衡量测序read唯一且可靠地比对到参考基因组某个位置的置信度。其数值范围通常为0-60，以Phred质量评分形式表示（即MAPQ = -10log₁₀(p)，其中p为比对错误的概率）。
- 可以表示比对的唯一性、可靠性。大于等于30可认为是唯一且准确的比对结果。
### (3.4)
通过SAM/BAM文件中的 ​MD标签（MD:Z）和​CIGAR字符串可推断出read映射区域的参考基因组序列。
- ​MD标签直接记录参考基因组的差异信息
​- CIGAR字符串结合比对起始位置确定参考序列覆盖的区间
- ​SEQ字段​（read序列）与MD标签结合，可重建参考序列
## (4)
- 直接下载bwa的.tar.gz文件
- 解压
```
gunzip bwa-0.7.19.tar.gz
tar -xvf bwa-0.7.19.tar
```
- 编译
```
make
```
- 建立index
```
 ./bwa index sacCer3.fa
```
- Mapping并输出为THA2-bwa.sam
```
./bwa mem sacCer3.fa THA2.fa > THA2-bwa.sam
```
## (5)Genome Browser
