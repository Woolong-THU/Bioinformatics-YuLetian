# Lecture 4
## R studio
很多不用自己写了，有很多R packages

<img width="795" alt="截屏2025-03-21 15 40 56" src="https://github.com/user-attachments/assets/bf147571-7d1d-4543-b902-95606e395199" />

## NGS(Next Generation Squencing)
### Seq tech
#### 1st gen. Sanger Seq
跑胶

#### 2nd gen. NGS

<img width="581" alt="截屏2025-03-21 15 50 33" src="https://github.com/user-attachments/assets/b5ef788b-a3cb-46f8-bc7c-286e3ca5518f" />

改进：Paired-end Sequencing<br>
如何处理数据？<br>

<img width="758" alt="截屏2025-03-21 15 53 47" src="https://github.com/user-attachments/assets/103f5a06-6573-4297-b6f6-24f6f3e0502e" />

会得到两个文件，每个文件内部序列方向相同<br>
Insert size代表打碎的片段长度<br>

<img width="734" alt="截屏2025-03-21 15 56 02" src="https://github.com/user-attachments/assets/a7d64294-5912-4e9c-ae65-66fb994b28f4" />

二代一般读长为2*150 bp，越长越容易出错<br>

#### 3rd gen. Single Molecule Real-time
- Pacific Biosciences
- Oxford Nanopore

### Types of NGS

<img width="312" alt="截屏2025-03-21 16 13 10" src="https://github.com/user-attachments/assets/ec1e64bb-5b49-430a-990b-13521d18e85b" />

#### Targeted Sequencing
如何获取想要的序列<br>
Hybrid VS Amplicon

<img width="792" alt="截屏2025-03-21 16 15 23" src="https://github.com/user-attachments/assets/ec06d27b-6966-44d4-a604-25d64cb3f463" />

### An example NGS study
外显子组测序,只需针对外显子区域即可，覆盖度更深，数据准确性更高，简便经济高效<br>
用于疾病致病基因、易感基因研究<br>

#### Data格式：FASTQ

<img width="518" alt="截屏2025-03-21 16 20 31" src="https://github.com/user-attachments/assets/e4954bf6-18bd-4ab9-beaa-dd1a66dd0831" />

#### 数据分析
1. Quality Control(FastQC)<br>
质量在绿色区域的可信，可以把不能用的去掉<br>

2. Reads mapping(Bowtie)

<img width="613" alt="截屏2025-03-21 16 23 11" src="https://github.com/user-attachments/assets/d80805c2-c413-497b-8208-3395742e305c" />

3. Mutations Identification

<img width="376" alt="截屏2025-03-21 16 23 53" src="https://github.com/user-attachments/assets/e79059bd-189d-48dc-948a-9eff84808612" />

4. Visualization
使用UCSC Genome Browser展示结果

### Reads Mapping
使用Bowtie，“多对一”<br>

#### Basic mapping prob.
- 允许错配

- Multiple locations
- Memory cost and speed

#### BWT(BW transformation)
BWT使用于表达数据更省空间，无损压缩信息<br>

#### Exact mapping
为什么mapping这么快？<br>
BWT可以快速移动指针<br>
Milestone<br>
Checkpoints<br>


#### Mapping allowing mismatch
允许错配数、多位置数均用默认即可<br>
