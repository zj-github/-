# crf 条件随机场的解析与应用
<br/>
## 随机场
在概率论中，由样本空间Ω = {0, 1, ..., G − 1}n取样构成的随机变量Xi所组成的S = {X1, ..., Xn}。若对所有的ω∈Ω下式均成立，则称π为一个随机场。```Ω(ω)>0```。现在已有一些随机场算法，如马尔可夫随机场（MRF），吉布斯随机场（GRF），条件随机场（CRF），和高斯随机场等。其中条件随机场可用于**序列标注**，本章主要介绍条件随机场（以下使用简写"crf"）。
<br/>
## 条件随机场
是随机场的一种，是一种判别式概率模型。(和判别式对应的是产生式模式)
<br/>



参考：
<a href="https://zh.wikipedia.org/wiki/%E9%9A%8F%E6%9C%BA%E8%BF%87%E7%A8%8B">随机过程</a>
<a href="https://zh.wikipedia.org/wiki/%E9%A9%AC%E5%B0%94%E5%8F%AF%E5%A4%AB%E6%80%A7%E8%B4%A8">马尔科夫性质</a>
<a href="https://zh.wikipedia.org/wiki/%E9%A6%AC%E5%8F%AF%E5%A4%AB%E9%81%8E%E7%A8%8B">马尔科夫过程</a>
<a href="https://zh.wikipedia.org/wiki/%E9%9A%90%E9%A9%AC%E5%B0%94%E5%8F%AF%E5%A4%AB%E6%A8%A1%E5%9E%8B">马尔科夫模型</a>
<a href="http://www.tanghuangwhu.com/archives/162">条件随机场（CRF) </a>
<a href="https://www.zhihu.com/question/35866596">如何用简单易懂的例子解释条件随机场（CRF）模型？它和HMM有什么区别？</a> 
