# Self-Attention 自注意力机制

句子辨识：

最简单的方法，开一个独热向量包含所有的可能词汇，有则记为1。

问题：未考虑到词汇之间的联系。

方法：25ms长度的信号，向前，后看10ms，以这样的形式表示一段声音讯号。这样，声音信号可以表述为一堆向量。

引申：Graph也是一堆向量的集合。



最终的公式：

![image-20220710112429484](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220710112429484.png)





进阶版本：Multi-Head Self Attention

“相关”可能有不同的形式，由不同的Q来表述，如图：



![image-20220710112613400](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220710112613400.png)

用两个q来做注意力处理，这样能够找到两种不同的相关性。

![image-20220710112843325](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220710112843325.png)





仍然存在的问题：没有考虑到词之间的位置远近信息。

为每个位置设计一个vector:ei，将ei加到ai上。  Positional Encoding有待研究，甚至可以学习出来。

自注意力机制的应用:Bert , Transformer...



自注意力可能会产生太大的矩阵，咋办？

Truncated Self-Attention 设定一个小一点的范围，而不是看整句话，以此可以加快运算速度。

应用在图像处理上：一张图像实际上可以看作是一个vector set。其和CNN的差异：CNN可以看作简化版的Self-A.



应用在Graph上时，只用计算有相连关系的点的分数即可。这是一种GNN模型。
