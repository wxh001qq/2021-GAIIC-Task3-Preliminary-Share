# 2021-GAIIC-Task3-Preliminary-Share

赛题 - 小布助手对话短文本语义匹配

非常开心拿到了初赛最后一周的周星星，目前线上成绩为0.919605。关注这个比赛很久了，但是由于工作的原因没法投入很多时间，也非常感谢前面几期周星星的无私分享，让我避免趟入很多坑。下面把自己的参赛心得分享给大家。

## 模型结构
目前线上最好成绩是nezha base + nezha large融合之后的结果。

## 数据增强
目前只用到了对偶 q1 - q2 = 1 => q2 - q1 = 1，闭包尝试了一下正负例都加入但是线下效果下降了，后面只加正例貌似有些提高，但是线上没有提交。这里对偶在预训练和微调的时候都用到了，闭包只是在微调的时候尝试了。

## 预训练
这里重点说一下预训练，其实感觉这题预训练是个很高的base，但是看群里很多同学反映预训练的分数不高。我总结一下可能的问题：
- 预训练的代码框架，最好还是参考之前的代码框架自己重新写一个，目前看群里开源的基于keras和pytorch的都有些不足，keras把预训练和cls分类集合在一起了，这样5折耗时太大。kaggle里pytorch的貌似把切词mask都集合在transformer框架里面，这个月改动mask策略也比较麻烦。
- mask策略，我直接用的ngram mask，具体mask策略代码可以参考google开源的albert
- 继续预训练，在mask之后继续用一些其他策略，比如全mask后半句，论文里面也有，我这里提升线下千分之1AUC
- warnup？因为自己做的时间比较少所以不确定有用不。

## 伪标签+蒸馏
因为复赛对时间有要求，融合模型上分不可行了，如何将大模型的能力迁移到小模型应该是决定复赛成绩的关键，目前我尝试了伪标签+蒸馏的方式。大概效果是单折0.908-》五折0.912-》蒸馏单折0.913

## 参考文献
- AI小花 - https://github.com/nilboy/reports/blob/master/gaic_track_3.md
- ch12hu - https://github.com/chizhu/tianchi-gaic-track3-share
- lololol - https://github.com/luoda888/2021-GAIIC-phase3-idea
- 猫老板 - https://gist.github.com/aloha12345x/b2b81c52dd5fee7b47e3a4eb537232f3
- 电费战士 - https://github.com/liucongg/2021-GAIIC-Task3-Share
- 哎哟wc - https://github.com/doubleQ2018/2021-GAIIC-Track3-share
- yzheng51 - https://github.com/yzheng51/2021-GAIIC-Task3-Preliminary-Share
