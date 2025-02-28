# Lecture 2
## Linux
### 简介
- 操作系统
- 个人用Ubuntu
### Shell & Bash
#### Shell
用户与Unix操作系统交互的界面。用户通过直接输入命令来执行各种各样的任务
#### bash
Linux的shell根据发展者的不同，有许多版本，比如Bourne Shell 、C Shell、K Shell等。Bash即Bourne Again SHell，是Bourne Shell（sh）的增强版本，在1987年由布莱恩·福克斯编写。它是GNU操作系统中重要的软件工具之一，目前也是GNU操作系统的基本shell。

<img width="327" alt="截屏2025-02-28 15 44 43" src="https://github.com/user-attachments/assets/e37b7b2e-5b32-4567-ac12-5c046d8c1b8e" />

## Intro 2. Higher Dimension
### 生信研究维度
- 1D-DNA
- 2D-RNA
- 3D-Protein
- HD-Human

<img width="340" alt="截屏2025-02-28 15 55 49" src="https://github.com/user-attachments/assets/51737143-a5a8-4be0-b988-999d26dbd45d" />


### 预测DNA/RNA序列
RNA World
#### HMM(Hiden Markov Model)
##### 找基因，预测Exon/Intron
软件：*GENSCAN*
##### Profile Gene Families,多序列比对
软件：*Pfam*
### RNA结构
#### 基于base pairing(RNA 二级结构)
- Trans-pairs
  - Interaction of 2 RNAs
- cis-pairs
  - Folding of 1 RNA
#### SCFG(概率Stochastic Context Free Grammar) for RNA 二级结构

<img width="502" alt="截屏2025-02-28 16 07 22" src="https://github.com/user-attachments/assets/c2fccdc4-8644-409d-85f5-458423bbdc2b" />

软件：*Rfam*
#### 三维结构 3DStructure Prediction
##### Transformer with Self-attention : context-free -> context sensitive
1. 捕捉到Long distance connection
2. Fast with parallel computing
例子：AlphaFold
软件：*3D Structurome*






