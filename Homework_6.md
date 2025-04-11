# Homework 6
## (1)
是单端测序结果。
因为<br>
```
0 + 0 paired in sequencing
0 + 0 read1
0 + 0 read2
```
无双端测序两端的reads统计。<br>
## (2)
Secondary alignment（次要比对）​ 是指同一条测序read被比对到基因组的多个位置时，除最优结果（primary alignment）外的其他可能比对位置。<br>
有4923条结果属于secondary alignment
## (3)
1. bedtools 安装<br>
根据教程在/home/test/samtools-bedtools下安装<br>
```
mkdir bin
wget -O bin/bedtools https://github.com/arq5x/bedtools2/releases/download/v2.30.0/bedtools.static.binary
chmod u+x bin/bedtools
echo "export PATH=$PATH:/home/test/samtools-bedtools/bin" >> ~/.bashrc
source  ~/.bashrc
which bedtools
chmod +x /home/test/samtools-bedtools/bin/bedtools
#运行时发现无权限故添加执行权限
```
2. 提取ACTB基因的gene区域并转换为bed格式，注意bed为 0-based
```
awk '$3 == "gene" && $9 ~ /ACTB/ { printf "%s\t%d\t%d\t%s\n", $1, $4-1, $5, "ACTB_gene" }' hg38.ACTB.gff > gene.bed
```
3. 提取所有exon区域并转换为BED格式
```
awk '$3 == "exon" && $9 ~ /Parent=.*ACTB/ { printf "%s\t%d\t%d\t%s\n", $1, $4-1, $5, "exon" }' hg38.ACTB.gff > exon.bed
```
4. 对exon区域排序并合并重叠或相邻的区间，避免重复扣除
```
bedtools sort -i exon.bed > exon.sorted.bed
bedtools merge -i exon.sorted.bed > exon.merged.bed
```
5. 计算intron区域，并以bed输出 intron.bed
```
bedtools sort -i gene.bed > gene.sorted.bed
bedtools subtract -a gene.sorted.bed -b exon.merged.bed > intron.bed
```
链接：https://github.com/Woolong-THU/Bioinformatics-YuLetian/blob/main/intron.bed
6. 提取比对到intron区域的reads
```
samtools view -b -L intron.bed COAD.ACTB.bam > intron_reads.bam
```
7. 将BAM转为FASTQ
```
samtools fastq intron_reads.bam > intron_reads.fastq
```
链接：https://github.com/Woolong-THU/Bioinformatics-YuLetian/blob/main/intron_reads.fastq

## (4)
1. 提取ACTB基因区域的reads
```
samtools view -b -L gene.bed COAD.ACTB.bam > ACTB_region.bam
```

2. 计算覆盖度并生成bedgraph
```
bedtools genomecov -ibam ACTB_region.bam -bg -split > ACTB_coverage.bedgraph
```
链接：https://github.com/Woolong-THU/Bioinformatics-YuLetian/blob/main/ACTB_coverage.bedgraph

3. 使用IGV可视化得到
<img width="1368" alt="截屏2025-04-06 12 58 30" src="https://github.com/user-attachments/assets/56bfbfb1-d0a4-4686-a725-9ed93a3c6501" />

## 简答题
### 1. 人类基因组的大小以及基本组成是哪些?
人类基因组大小为：3,099,750,718 bp （核基因组）<br>
基本组成：
<img width="1368" alt="image0" src="https://github.com/user-attachments/assets/fcb5061c-86b8-431a-b21d-281e2b4e0d3a" />

| 组成类型               | 数量              |
|------------------------|-------------------|
| Coding genes           | 19,868            |
| Non coding genes       | 42,160            |
| Small non coding genes | 4,867             |
| Long non coding genes  | 35,076            |
| Misc non coding genes  | 2,217             |
| Pseudogenes            | 15,206            |
| Gene transcripts       | 387,944           |
<br>
<img width="1368" alt="image1" src="https://github.com/user-attachments/assets/ef710461-9067-4289-a128-794363acbf7a" />
（数据来源：Ensembl GRCh38.p14 更新：Jul 2024）<br>

### 2. 基因中的非编码 RNA的最新注释是多少个了?请详细列一下其中的非编码 RNA 的细分类型的数目，并对主要的非编码 RNA 是什么做的用1-2句解释一下。
非编码RNA最新注释43499个。(Gencode v47,10.2024,GRCh38.p14)
| region        | source        | # of genes | # of transcripts | annotation file         | 
|---------------|---------------|------------|------------------|-------------------------|
| rRNA| [gencode v47](https://www.gencodegenes.org/human/stats_47.html)| 47        | 47       | gencode.v47.annotation.gtf.gz   | 
| tRNA     | [gencode v47](https://www.gencodegenes.org/human/stats_47.html)   | 649    | 649  | gencode.v47.annotation.gtf.gz  |
| snRNA    | [gencode v47](https://www.gencodegenes.org/human/stats_47.html)   | 1901     | 1901 |gencode.v47.annotation.gtf.gz  | 
| snoRNA   | [gencode v47](https://www.gencodegenes.org/human/stats_47.html)   | 942 | 942| gencode.v47.annotation.gtf.gz    |
| miscRNA    | [gencode v47](https://www.gencodegenes.org/human/stats_47.html)   | 2208        | 2208         | gencode.v47.annotation.gtf.gz   |
| lncRNA   | [gencode v47](https://www.gencodegenes.org/human/stats_47.html)   | 34914 | 189177 | gencode.v47.annotation.gtf.gz |
| miRNA   | [gencode v47](https://www.gencodegenes.org/human/stats_47.html)   | 1879 | 1879 | gencode.v47.annotation.gtf.gz |
| scaRNA   | [gencode v47](https://www.gencodegenes.org/human/stats_47.html)   | 49 | 49 | gencode.v47.annotation.gtf.gz |
| scRNA   | [gencode v47](https://www.gencodegenes.org/human/stats_47.html)   | 1 | 1 | gencode.v47.annotation.gtf.gz |
| sRNA   | [gencode v47 ](https://www.gencodegenes.org/human/stats_47.html)  | 5 | 5 | gencode.v47.annotation.gtf.gz |
| ribozyme   | [gencode v47 ](https://www.gencodegenes.org/human/stats_47.html)  | 8 | 8 | gencode.v47.annotation.gtf.gz |
| piRNA   | [piRbase v3](http://bigdata.ibp.ac.cn/piRBase/) (2021)   | 77242 | 77242 | hsa.pirbase.gold.v3.0.fa.gz |
| cicrRNA   | [CIRCpedia v2](http://yang-laboratory.com/circpedia/)   | 183943 | 183943 |   |
