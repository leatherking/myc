# Generative Feature Replay For Class-Incremental Learning

## Abstract
1. 新旧类别有imbalance，导致网络偏向新类
2. 生成重放对于简单数据集有效，复杂数据集仍然很难
3. 提出生成特征重放解决imbalance问题，不需要exemplar。将生成特征重放和分类器合在一起，使用特征级蒸馏
4. 在CIFAR-100和ImageNet上实验，空间占用少

## Intro & Related
1. 三类方法：正则化方法，限制可塑性；动态网络，结合任务独立的mask；记忆重放，rehearsal & pseudo-rehearsal
2. **在ZSL和FSL中，生成特征的方法达到了很好的效果，可以关注一下这方面的论文。**

## 遗忘分析（CCA）
1. [典型相关分析（CCA）](https://zhuanlan.zhihu.com/p/52110862)
2. 

## Conclusion
1. 生成特征重放与特征蒸馏结合
2. 通过CCA显示灾难性遗忘在不同层的表现
3. 高层特征比像素级更简单，可以使用更简单的生成器
4. 比其他不用exemplar的方法好很多
5. 在一些设置下比exemplar方法好
6. 生成器空间占用小