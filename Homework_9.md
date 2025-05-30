# Homework 9
## (1)
1. 可以通过Input对照排除背景噪音，排除因本底表达水平高或一些非特异性结合所造成的假阳性peaks
2. 可以验证染色质断裂的效果，比如观察其电泳条带等情况来判断断裂是否达到预期
3. 若根据 Input 中的 DNA 靶序列的含量和 IP 后样本中的靶序列含量，按照取样比例进行换算，还能够间接推算 ChIP 实验的效率
## (2)
### findPeaks​
1. -style<br>
​定义峰值检测模式，根据实验类型选择算法<br>
​参数值：<br>
​**factor**：适用于转录因子结合位点的检测。此模式假设结合位点信号集中且峰形尖锐，通过统计显著性筛选高置信度峰。<br>
​**histone**：适用于组蛋白修饰的宽峰检测，允许可变长度峰区，并整合信号连续性分析。<br>

​2. -o<br>
​指定输出文件路径，默认生成包含峰位置、富集分数和统计信息的文本文件<br>

3. ​-i<br>
输入对照样本的 tag directory，用于背景噪音校正<br>
### findMotifsGenome.pl<br>
​1. -len<br>
​功能：定义 Motif 的碱基长度，支持多值输入（逗号分隔）<br>
​默认值：8,10,12，适合大多数转录因子结合位点的识别<br>
若研究长 Motif（如复合调控元件），可设为 -len 15,20<br>
​2. -size<br>
​功能：设定用于 Motif 分析的序列窗口大小<br>
​默认值：200<br>
​3. -S<br>
​功能：指定输出的 Motif 数量
​默认值：25，通常足以覆盖主要富集 Motif<br>

## (3)
len = 8<br>
<img width="1191" alt="截屏2025-05-03 11 05 15" src="https://github.com/user-attachments/assets/70ea4fdd-4972-4316-af91-3e8a690ce733" />

过滤后的peaks文件如下：
```
PeakID	chr	start	end	strand	Normalized Tag Count	focus ratio	findPeaks Score	Total Tags	Control Tags (normalized to IP Experiment)	Fold Change vs Control	p-value vs Control	Fold Change vs Local	p-value vs Local	Clonal Fold Change
chrIV-1	chrIV	465220	465468	+	111129.9	0.920	15510.000000	15585.0	234.1	66.57	0.00e+00	55.11	0.00e+00	0.50
chrIV-2	chrIV	1490100	1490348	+	81687.8	0.857	11468.000000	11456.0	195.1	58.72	0.00e+00	35.06	0.00e+00	0.50
chrV-1	chrV	141138	141386	+	54449.0	0.855	7647.000000	7636.0	182.3	41.88	0.00e+00	21.55	0.00e+00	0.52
chrV-2	chrV	69078	69326	+	48659.0	0.823	6837.000000	6824.0	206.5	33.05	0.00e+00	20.52	0.00e+00	0.50
chrV-3	chrV	85195	85443	+	46277.4	0.861	6493.000000	6490.0	225.6	28.77	0.00e+00	21.56	0.00e+00	0.50
chrIV-3	chrIV	1080509	1080757	+	34405.0	0.832	4830.000000	4825.0	234.1	20.61	0.00e+00	23.11	0.00e+00	0.50
chrIV-4	chrIV	599953	600201	+	26597.0	0.755	3733.000000	3730.0	190.1	19.62	0.00e+00	15.58	0.00e+00	0.50
chrV-4	chrV	321939	322187	+	24821.5	0.754	3484.000000	3481.0	177.4	19.63	0.00e+00	13.66	0.00e+00	0.50
chrIV-5	chrIV	1468786	1469034	+	23595.0	0.794	3317.000000	3309.0	193.7	17.08	0.00e+00	14.36	0.00e+00	0.51
chrIV-6	chrIV	132817	133065	+	19402.3	0.782	2723.000000	2721.0	209.3	13.00	0.00e+00	11.95	0.00e+00	0.52
chrIV-7	chrIV	591669	591917	+	18304.2	0.792	2568.000000	2567.0	200.1	12.83	0.00e+00	12.88	0.00e+00	0.50
chrIV-8	chrIV	721812	722060	+	17840.7	0.739	2514.000000	2502.0	192.3	13.01	0.00e+00	9.61	0.00e+00	0.51
chrIV-9	chrIV	1	230	+	17206.1	0.884	2444.000000	2430.0	60.3	40.30	0.00e+00	632.81	0.00e+00	0.81
chrIV-10	chrIV	1233763	1234011	+	16250.6	0.888	2303.000000	2279.0	156.1	14.60	0.00e+00	10.68	0.00e+00	0.52
chrIV-11	chrIV	234340	234588	+	16179.3	0.790	2275.000000	2269.0	199.4	11.38	0.00e+00	9.37	0.00e+00	0.51
chrV-5	chrV	225453	225701	+	15066.9	0.901	2123.000000	2113.0	157.5	13.42	0.00e+00	11.73	0.00e+00	0.53
chrIV-12	chrIV	357166	357414	+	15052.6	0.705	2113.000000	2111.0	205.7	10.26	0.00e+00	8.19	0.00e+00	0.51
chrIV-13	chrIV	416932	417180	+	13954.5	0.838	1973.000000	1957.0	188.0	10.41	0.00e+00	10.83	0.00e+00	0.51
chrIV-15	chrIV	1278678	1278926	+	13697.8	0.852	1925.000000	1921.0	225.6	8.51	0.00e+00	12.86	0.00e+00	0.52
chrIV-16	chrIV	1164971	1165219	+	13419.7	0.746	1887.000000	1882.0	215.7	8.73	0.00e+00	9.29	0.00e+00	0.52
chrV-6	chrV	491091	491339	+	12457.1	0.773	1749.000000	1747.0	180.9	9.66	0.00e+00	13.69	0.00e+00	0.52
chrIV-14	chrIV	1525285	1525496	+	11779.7	0.923	1953.000000	1652.0	58.2	28.40	0.00e+00	32.51	0.00e+00	1.27
chrIV-17	chrIV	722439	722687	+	10774.3	0.676	1512.000000	1511.0	172.4	8.76	0.00e+00	5.26	0.00e+00	0.53
chrIV-46	chrIV	568825	569073	+	5005.7	0.820	705.000000	702.0	85.8	8.18	0.00e+00	4.54	3.86e-226	0.84
```
在TSS附近的分布：
<img width="484" alt="截屏2025-05-03 12 14 21" src="https://github.com/user-attachments/assets/4e8f8f92-0dac-435f-8654-31c7b37c6862" />
