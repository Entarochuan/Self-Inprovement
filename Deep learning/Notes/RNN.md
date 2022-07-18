# RNN

Recurent：系统知道那个词汇属于哪个slot。

首先，将一个词汇表示成一个vector， 输入后返回其属于某个slot的几率。但是问题在于一个词可能以不同的词性出现。这就要求网络具有记忆，可以联系上下文:RNN

将每次的output都存到memory里面，下一次input时也考虑储存的值。

具体使用：

![image-20220710153819349](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220710153819349.png)

首先初始化memory，全0， 输入第一个量(1,1)， 得到output(4,4)，网络存储绿色节点的值。

![image-20220710153959572](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220710153959572.png)

再输入1,1 ， 这时记忆会再反馈到绿色节点上，对下一个结果作贡献。这样确保了，在同样输入的情况下可以得到不同的输出。对于实际的情况，可以得到如下的流程:

![image-20220710154238899](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220710154238899.png)



网络也可以设计为深层的。



变式：Jordan Network

存储整个网络的输出作为输入。



### Bidirectional RNN

双向RNN:同时在双向训练网络。好处：网络看的范围比较广。

![image-20220710154832955](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220710154832955.png)





## LSTM     long short-term memory

记忆的写入由input gate控制，闸门的控制是由网络自己学习的。

 再提供一个forget gate，网络自己学习什么时候遗忘学习的信息。

故为：较长规模的短时记忆网络。

LSTM的一个记忆单元结构图：

![image-20220710155756235](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220710155756235.png)

一般f选用sigmoid，以表征gate被打开的程度。

forget gate 打开时视作记得，关闭时视作遗忘。

四个输入的点都接受不同的输入(即输入网络的值)，在每个阶段作乘法，进行数据的处理。



故，只需把Neural换成LSTM的cell即可。更换之后，实际上LSTM多出了四倍的参数量。



具体的优化流程:

![image-20220710161556818](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220710161556818.png)

首先，参数用C表示。

输入X经过transform得到Z作为每一个LSTM元的输入。将X乘上四个不同的transfrom得到四个不同的输入，操控memory cell的运作。![image-20220710161827324](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220710161827324.png)



根据cell的定义，得到以下的流程:

![image-20220710162019292](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220710162019292.png)



这样就完成了输入-记忆-输出的流程。 

将这样的结构堆叠形成多层LSTM。

![image-20220710164724253](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220710164724253.png)



怎样训练RNN?

Loss:

定义为到标签((Reference Vector)的距离。

仍然使用Gradient descent， 参数的更新为BPTT(这里不讲力)

RNN的训练其实是比较困难的。RNN对参数的变化是非常陡峭的，在某些地方很陡峭，在另一些地方很平坦。

做法：限制最大的Gradient即可。



#### 为什么LSTM可以解决Gradient Vanishing的问题？

RNN在每个时间点都会覆盖memory， LSTM则会将此刻与以前的值相加，以往gradient的影响会持续，因此过去的影响会持续，这样就能解决问题。



在多数情况下要保持forget gate的开启，以确保记忆的持续性。



另一种模型:GRU ---- gate recurrent unit，当input打开,forget关闭，只有记忆被清楚才重新记忆，这比LSTM要简单。



### RNN适合处理的问题

1. input-one

2. input-output,其中输出比输入要短 trimming拿掉重复的部分



问题在于，区分不了好棒和好棒棒(乐

区分的方法:CTC。在output时多输出一个符号null表示没有信息，这样就能得到:

![image-20220710185727994](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220710185727994.png)

怎样训练？

![image-20220710185816712](C:\Users\YichuanMa\AppData\Roaming\Typora\typora-user-images\image-20220710185816712.png)

穷举所有可能的好，棒的分布可能。算法这里不细讲。 



### 再:Seq2Seq

如将英文句子翻译成中文句子:Mechine Learning ---- 机器学习

然而可能的问题是，模型并不知道应该在什么地方停止。需要多提供一个中断符号表示停止。

用这样的技术可以做到：另一种语言的语音——这一种的文字



另：阅读理解/图片解释/听力测试



Q: RNN 和 Structured Learning 之间的关系？

RNN只能看见序列的一部分，而后者能观察整个序列。但bidirectional RNN也可以做到这一点。

后者能够更好的满足一些特殊label需求。但是RNN可以是deep的，这使RNN,LSTM具有优势。



更好的办法：将CNN/LSTM/DNN与HMM结合使用。