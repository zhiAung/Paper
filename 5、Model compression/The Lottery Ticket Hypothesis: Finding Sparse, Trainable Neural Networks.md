# 一、论文
The Lottery Ticket Hypothesis: Finding Sparse, Trainable Neural Networks

https://arxiv.org/abs/1803.03635
# 二、论文笔记
1、摘要：神经网络剪枝技术可将网络参数量减少 90%，进而在不牺牲准确率的前提下减少存储需求、提升推断的计算性能。然而现有经验表明，剪枝生成的解析架构从一开始就很难训练，尽管解析架构同样可以提升训练性能。
我们发现，标准的剪枝技术会自然地发现子网络，这些子网络经过初始化后能够有效进行训练。基于这些结果，我们提出了「彩票假设」（lottery ticket hypothesis）：密集、随机初始化的前馈网络包含子网络（「中奖彩票」），当独立训练时，这些子网络能够在相似的迭代次数内达到与原始网络相当的测试准确率。
「中奖彩票」赢得了「初始化彩票」：它们的连接具有使训练非常高效的初始权重。我们提出了一种识别中奖彩票的算法，并用一系列实验来支持彩票假设以及这些偶然初始化的重要性。我们发现在 MNIST 和 CIFAR10 数据集上，「中奖彩票」网络的大小不及全连接、卷积前馈架构的 10%-20%。而且，这种「中奖彩票」比原始网络学习速度更快，测试准确率也更高。


2、确定 中奖彩票的两种策略（第一个效果更好）

（a）、
Strategy 1: Iterative pruning with resetting.
1. Randomly initialize a neural network f(x; m  θ) where θ = θ0 and m = 1|θ|
is a mask.
2. Train the network for j iterations, reaching parameters m  θj .
3. Prune s% of the parameters, creating an updated mask m0 where Pm0 = (Pm m s)%.
4. Reset the weights of the remaining portion of the network to their values in θ0. That is, let
θ = θ0.
5. Let m = m0
and repeat steps 2 through 4 until a sufficiently pruned network has been
obtained.

每次修剪完之后，剩下的节点的权重置为初始化权重，然后接续迭代修剪

（b）、
Strategy 2: Iterative pruning with continued training.
1. Randomly initialize a neural network f(x; m  θ) where θ = θ0 and m = 1|θ|
is a mask.
2. Train the network for j iterations.
3. Prune s% of the parameters, creating an updated mask m0 where Pm0 = (Pm m s)%.
4. Let m = m0
and repeat steps 2 and 3 until a sufficiently pruned network has been obtained.
5. Reset the weights of the remaining portion of the network to their values in θ0. That is, let
θ = θ0

每次修剪完之后，剩下的节点的权重不变接着训练，然后接续迭代修剪

最终训练完之后，两者都是把剩余节点的权重置为最初始的权重。

剪枝策略：
We use a simple layer-wise pruning heuristic: remove a percentage of the weights with the lowest magnitudes within each layer
（Song Han, Jeff Pool, John Tran, and William Dally. Learning both weights and connections for efficient neural network. In Advances in neural information processing systems, pp. 1135–1143,2015.）

3、一些观点

（a）、从一开始就训练一个修剪过的网络比重新训练一个修剪的网络效果更差

（b）、网络修剪之后，剩余的节点（剩余节点组成的模型就是中奖彩票）最好使用最开始的初始化权重，而不是重新随机化权重。（这也是本论文的一个核心实验），表明单靠网络结构是不能表明这种假设的的有效性的，言外之意还需要初始化的参数

（c）未经验证的新的猜想：

The Lottery Ticket Hypothesis. A randomly-initialized, dense neural network contains a subnetwork that is initialized such that—when trained in isolation—it can match the test accuracy of the
original network after training for at most the same number of iterations.

SGD在所有的初始化权重里边找到了一个较好初始化的权重子集组成的网络结构，随机初始化的密集网络（完整网络）比通过剪枝产生的稀疏网络更容易训练的原因是，密集网络有更多的可能的子网络可以恢复出”中奖彩票“

4、Contributions

Contributions.
• We demonstrate that pruning uncovers trainable subnetworks that reach test accuracy comparable to the original networks from which they derived in a comparable number of iterations.
• We show that pruning finds winning tickets that learn faster than the original network while
reaching higher test accuracy and generalizing better.
• We propose the lottery ticket hypothesis as a new perspective on the composition of neural
networks to explain these findings.


5、思想可以带来的好处（虚）

（a）、改善训练策略，以及训练效果

（b）、设计更好的网络结构

（c）、对神经网络更好的理论理解

6、LIMITATIONS AND FUTURE WORK

On deeper networks (Resnet-18 and VGG-19), iterative pruning is unable to find winning tickets
unless we train the networks with learning rate warmup. In future work, we plan to explore why
warmup is necessary and whether other improvements to our scheme for identifying winning tickets
could obviate the need for these hyperparameter modifications.

在比较深的网络中，”中奖彩票“不容易寻找，除非使用大的学习率进行热身训练。这一部分还待探讨

7、实验非常的全面以及详细，从实验以及参考文献也可以看出来作者对这方面的研究颇深，理论丰富，这或许也是论文能得奖的原因。
