# Transfromer

Seq2Seq任务，不知道输出序列的长度是多少。



是否可以输入一种语言的声音讯号，输出另一种语言的文字？

训练资料：语音+字幕

也可以用来训练聊天机器人。这其实就是一个Q--A的任务。往往为任务特制化模型表现会更好。

![image-20220712153657102](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220712153657102.png)



Multi-label 分类问题：同一个数据有多个标签。

思路1：分类问题取最大的几个标签。然而不行，因为每个文章有多个标签。

这可以用Seq2Seq硬做！

### Seq2Seq的结构：

![image-20220712154430219](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220712154430219.png)

### Transforemer的架构:

![image-20220712154448009](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220712154448009.png)

Encoder的任务：给一排向量，输出一排向量，如RNN,CNN,Self-Attention，这里使用自注意力。

在transformer中，输出的vector再与原本的输入相加，综合得到新的输出。这样的架构称为:

<u>Residual Connection</u>，再作**normalization,**计算输入向量的均值和标准差，作标准化。

![image-20220712154912083](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220712154912083.png)



将以上步骤后得到的结构作为Fully Connected Network的输入，全连接网络的结果再作残差链接，再标准化。

这样设计的架构即为transformer的encoder部分。



### Decoder

decoder的主要流程：读取encoder的输入，给出一个符号表示开始输出，

规定Vocabulary Size，如常用汉字(几千个)，不同语言有不同的输出单位。

![image-20220712155803934](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220712155803934.png)

输出的黄色向量即为常用词表的分布，分数最高的字作为输出。

接下来将机和encoder作为共同的输入，得到新的输出。

![image-20220712155912834](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220712155912834.png)

decoder会把自己的输出当作接下来的输入值，所以有可能会看到错误的输入。

![image-20220712160124879](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220712160124879.png)

其中Self-Attention加上了Masked，意思是：产生bi的时候只能看a1...ai的信息，只拿左边的信息生成。

Why？**因为Decoder的输入是一个一个一个输入的(悲**



还剩下的问题是，怎样知道输出序列的长度(什么时候输出停止符号)。

准备一个停止的符号**END**来表示结束。

decoder类型分成AT和NAT

![image-20220712161134901](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220712161134901.png)

NAT需要网络决定输出长度，或者是找到第一个end的位置。

NAT的好处是可以并行运算。但是往往表现一般，存在Multi-Modality的问题。



### Encoder-Decoder信息传递？

![image-20220712161351978](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220712161351978.png)

Cross-Attention用来连接两个模块。



训练中，标签是一个独热向量，其实就可以看作一个分类任务。最小化总的交叉熵损失。





### Copy Mechanism:

聊天机器人任务：

![image-20220712162643785](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220712162643785.png)

 

机器有时存在漏字的问题，解决:Guided Attention，使Attention有一个固定的样貌。



### Beam Search

![image-20220712172158798](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220712172158798.png)

在生成树上找到似然度最大的路径。BFS

这样的方法有时是没有用的。

需要创造性的任务/语音合成的任务在模型中需要加入随机噪声。





### 各种的Self-Attention变式

