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
1. [典型相关分析（CCA）](https://zhuanlan.zhihu.com/p/52110862)，计算特征分布的相似度
2. ![GFR-IL_figure2](GFR-IL_figure2.png?raw=true "CCA")
    $M_t$提取task $t'<t$ 的特征与训练完的最佳模型 %M_{t'}% 的相似度

    - Finetune: 高层特征更加task相关，相似度下降多；底层特征概括性更强

    - LwF：跟Fine-tune差不多，蒸馏损失限制中间特征相关性能力太弱

    - 生成图片重放：真实图片是高维表示，特定任务的图像分布是窄而复杂的流形。难训、计算量大、需要大量训练图、依赖初始化、质量低。跟前两个一样，中间层相关性下降。
    
    - Feature distill：$F_{t-1}$和$F_t$之间的MSE有效减少遗忘

    - 

## Method
1. ![method](GFR-IL_figure1.png?raw=true "feature replay")
2. 模型包含：classifier、feature extractor feature generator
3. ![framework](GFR-IL_figure3.png?rwa=true "framework of GFR-IL")
    - 用feature extractor的特征训练classifier
    
    - 训练完feature extractor和classifier后冻结，用特征训练generator

    - 用生成特征进行重放，两种方式：1）学习各个类的高斯分布；2）MeRGAN方式

    - 蒸馏部分：$F_{t-1}$和$F_t$之间，$G_{t-1}和$G_t$用MeRGAN的蒸馏alignment，都是MSE


## 实验部分
1. 数据集：
    - ImageNet-100（前100类，打乱顺序，224×224训练时random crop，测试时center crop）
    - CIFAR100（padding 4，训练时random crop，测试时center crop）
    - 水平翻转
2. 训练
    - CIFAR100 resnet18，分类任务201 epochs，GAN 501 epochs
    - ImageNet-100 resnet18 分类101 epochs，GAN 201 epochs
    - Adam 学习率分类1e-3，GAN 1e-4
3. 测试
    - 平均准确率 同iCaRL和LUCIR
    - 平均以往率 同RWalk
4. 比较
    - ImageNet-100 ![实验1](GFR-IL_figure4.png?raw=true "imagenet-100")
        - 高斯采样生成比iCaRL-NME和CNN都好很多
        - 随着任务数增多，比LUCIR好得更明显
    - CIFAR100 ![实验2](GFR-IL_figure5.png?raw=true "cifar-100")
        - 5task、10task达到最好
        - 25task比LUCIR略差
    - 空间占用 ![实验3](GFR-IL_table1.png?raw=true "storage")
        - MeRGAN的GAN占用 8.5MB，GFR-IL占用4.5MB（ImageNet和CIFAR100都）

## Conclusion
1. 生成特征重放与特征蒸馏结合
2. 通过CCA显示灾难性遗忘在不同层的表现
3. 高层特征比像素级更简单，可以使用更简单的生成器
4. 比其他不用exemplar的方法好很多
5. 在一些设置下比exemplar方法好
6. 生成器空间占用小