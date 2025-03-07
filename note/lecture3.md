# Lecture 3 
## Sequence alignment-Blast
### Scoring matrix

<img width="714" alt="截屏2025-03-07 15 42 44" src="https://github.com/user-attachments/assets/64043da1-fe3d-4424-b0b6-e6759e2cba1f" />
<img width="913" alt="截屏2025-03-07 15 44 40" src="https://github.com/user-attachments/assets/5d0108d5-9e49-44e2-b2bc-8a598c3a063d" />
也可以比对氨基酸序列，有对称/不对称矩<br>
匹配/错配/空位

### Needleman-Wunsch Algorithm (Global)
用到了动态规划算法dynamic programming。是全局(Global)算法，比对整个序列。追求整体最高分。

<img width="809" alt="截屏2025-03-07 15 52 28" src="https://github.com/user-attachments/assets/0a40e009-3fe3-4e8e-82fd-7f525c9559c2" />
有时忽略motif

<img width="855" alt="截屏2025-03-07 15 59 24" src="https://github.com/user-attachments/assets/0fbf5744-3ce7-4969-b7c0-b2dda8d11ea3" />
回溯时只能斜着走<br>

### Smith-waterman Algorithm (Local)

<img width="818" alt="截屏2025-03-07 16 03 14" src="https://github.com/user-attachments/assets/c8d07279-4555-48f4-822d-69efc7b8c399" />
目标：sub-strings of highest similarity.<br>
不一定需要比对整个序列。

<img width="841" alt="截屏2025-03-07 16 04 24" src="https://github.com/user-attachments/assets/284211e9-e3fe-4875-9a82-42f463cdf267" />

### Application-BLAST

<img width="881" alt="截屏2025-03-07 16 29 55" src="https://github.com/user-attachments/assets/38f64ab4-fd98-4145-8d60-71e37a692d2d" />
<img width="874" alt="截屏2025-03-07 16 30 20" src="https://github.com/user-attachments/assets/724a6bc6-71b1-4ed0-80f9-7deaf49fa95b" />

- Raw score：原始分，不怎么有用
- bits score：修正后分数，平衡了序列长度的影响，可以比较用

<img width="855" alt="截屏2025-03-07 16 22 14" src="https://github.com/user-attachments/assets/7f9d557e-10ff-4a78-9616-853588511751" />

```
什么是 multiple testing?
```

<img width="904" alt="截屏2025-03-07 16 24 03" src="https://github.com/user-attachments/assets/341d4e90-a743-4079-b543-bf6039164696" />

可以看一下blast变体<br>
<img width="795" alt="截屏2025-03-07 16 24 45" src="https://github.com/user-attachments/assets/53f8a2fb-4527-48e3-8f01-2a54db404147" />

## Conservation Analysis(保守性分析)

homologs,paralogs,orthologs

<img width="914" alt="截屏2025-03-07 16 39 05" src="https://github.com/user-attachments/assets/fd710f53-4d5e-40f6-a120-63d3678cefb2" />

## Genomics projects & resources
略
