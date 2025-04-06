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
链接：


