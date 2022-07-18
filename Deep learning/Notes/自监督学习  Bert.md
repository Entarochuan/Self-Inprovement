# 自监督学习  Bert

Bert 模型很大，有340m的参数。

其他的大模型: ELMO, BERT, GPT-2, Megatron, T5, GPT-3, Switch-Transformer  大小呈递增趋势。



## Bert系列模型

首先介绍自监督学习。自监督学习不提供标签信息。找一个方法把未标注资料区分为训练集和测试集。

Bert本身是一个Transformer的Encoder，一般用在自然语言处理，输入为一排文字。

### 1.Masking Input

随机对输入进行掩蔽。 (首先选择遮蔽的字，再选择遮蔽的方式，加入特殊符号或者是换成随机字)

再将输出经过Softmax和Linear、Softmax，得到一个分布。

![image-20220714152918700](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220714152918700.png)

最小化被遮蔽的位置的向量和原来点的独热向量差距即可。这也就是一个分类问题。

训练中训练的是Bert和Linear model， 二者一起训练，输出对被盖住词汇的预测。

#### 应用1：Next Sentence Prediction

输入两个句子，判断两句是不是相连的结构，是则输出yes.这个任务对训练Bert没啥用。



## 应用：Downstream Tasks

<u>预训练</u>的Bert可以被用在其他任务上。这样的操作称为Fine-tune。

下面介绍使用Bert的具体案例。



1.判断句子情感色彩正面还是负面 

![image-20220714154747882](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220714154747882.png)

在结合训练时需要一定的标注资料。故整体的模型是Semi-Supervised。



2.词性标注类问题(输入输出同一长度)

![image-20220714155440600](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220714155440600.png)



3.从前提能否推出假设(自然语言逻辑处理)

![image-20220714155740347](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220714155740347.png)

输出两个输入句子之间的关系。

应用：立场分析。具体的解决形式:

![image-20220714155821817](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220714155821817.png)

只取cls的交叉输出结果，输入线性模型进行分类。



4.问答系统 Q&A

答案一定出现在文章中。

![image-20220714160042582](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220714160042582.png)

输入文章，问题，这两个显然都是序列。输出两个正整数，截取文章中两个正整数的区间作为正确答案。

解决办法：应用预训练的Bert模型，

![image-20220714160459100](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220714160459100.png)

预测正确答案的位置即可。
