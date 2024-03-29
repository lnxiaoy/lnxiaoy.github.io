---
title: 我与RTT的这三年
date: 2021-11-15 22:11:56
tags: 工作
---

已转岗3个月，特写下这篇文章，总结了3年的工作经历和态度变化，待之后回顾这段时间的心态和做出的选择。

### 工作经历

    由于本人有一年的社会工作经历，这个经历需要后面单独写一写了。
    进入部门后先后从事了数据比对、外场问题联调、性能定标、算法方案分析，路线还算清晰，但并不是每件事情做完就结束了，它时时刻刻在干扰着你，让你回顾历史方案，回顾历史性能，比较心累。另一方面，随着人员流动，上面的位置空了出来却发现自己在这个框架和体制下并没有发展空间了。所以选择了转岗，目前只负责一个方向的事情，做完了就可以传递到下一个环节，自己可以投身到其他领域上继续工作了。

### 存在的问题

    这一部分主观意见比较多，也是和同事讨论了几次汇总下来的。

1. 绩效导向不明确
    RTT本应是一个交付组织，可以交付版本上的事情，可以把芯片上的事情汇总分析，但却把一部分研究的事情承担了起来。这部分事情一方面是交付过程中发现的，可以通过调整实现过程中的细节做到的；一方面是外部需求输入，需要配合在版本和样机中验证的；另一方面是给自己找的KPI，如果减少复杂度、提高接收端信噪比之类的。本身三方面都可以做，但是要有轻有重，根据比例来做绩效导向。但是现实是我们需要支撑老大的绩效，老大的绩效里并没有一项是保证版本交付质量的实施细节，更多的反而是每年性能10%，容量10%，覆盖10%等等的任务令和断裂点。这样的话，员工在下面做的细枝末节的工作并没有呈现，也没有得到认可。由此可见也没有正向的激励循环。
2. 人员短缺
    在部门的梯队建设方面，内忧外患。外部们的需求较多，基本是一人负责一个，AB版本轮一次或者组与组之间轮一次。在RTT内部基本就是一个人扛着外面一个组的压力，老大居然还认为这是很正常的事情，不可理解。这直接导致了你工作时间的拉长，工作方向的增多，细枝末节的事情繁冗复杂，时间长了不堪重负。并且随着人员的流动，骨干人员越来越少，工作交接下来的事情越来越多，直接进入一个倒退的循环。
3. 压力下发，层层加码
    很多时候涉及版本过点，压力传递到部门内部的时候并没有专家或者主管出来“消消毒”，说这可以怎么怎么分析，你们内部可以先闭环之类的。尤其到芯片版本阶段，性能扣的很细，但并没有什么卵用。一个很形象的例子是，你在干活，前面众多老大的意见是abcd好几个方向，你不知道哪个对，都需要时间来验证；屁股后面是版本经理催着、大版本经理通报着、邮件吵架着；上面是你想落这个方案，你想做这个决策的天花板，他不让你自己决定，让你花一天时间来写报告汇报，让老大的老大来拍。总而言之除了时间上的付出导致身体疲惫，还有心里上的压抑，一声声叹气都缓解不了的压抑。
4. 氛围封闭，工具封闭
    本来身为交付环节中的一环，却只能埋头苦干，没有途径也没有机会接触一些新鲜算法、新颖的思路。代码在红区运行，性能和工具也只在部门内部互相传递，方案交流更多的时间表现为了自己人在打自己人。

### 一些想法

1. 光环
    在一个垂直行业较深的领域，有一个较高的学历、有一个资深的研究背景、有一个口碑较好的老板，在工作的时候都会使得你有一个光环，这个光环可以让你起步比别人快一些，在别人或者老大的印象里更深刻一些。如果想公平竞争的话，就得考虑一些其他行业，不是高精尖技术领域的，不需要太多的技术积累的，不需要太多经验沉淀的行业。相比较而言，这些行业普遍是短期热门，但比较容易乘上快车，实现原始的财富积累。老一辈人说的“越老越吃香”的行当可能就是前者吧。
2. 标签
    团队中或者平时交往中，总会发现有些人是舆论的中心，言论的焦点，信息的源头和传播者，往往这些人也具有一定的人格魅力和各式各样的标签，比如：爱吃炸鸡、C语言专家等等。有时候可以刻意塑造下这种形象，可以引来更多的关注，或者更多的机会，或者更多的朋友。
3. 跨界
    现在这个社会打败你的往往不是同行，而是跨界。我可以是唱歌行业里最懂相声的，也可以是相声行业里最懂唱歌的，虽然都不是强项，但在不同的圈子里还是可以露脸的。
4. 冒险
    年轻的时候还是值得去冒险，值得去尝试一些有风险的事情。因为现在的年纪本身负担小、走弯路代价小，总体来说可以承受起。
5. 人不能太“老实”
    “老实”的意思是不能按部就班，别人安排给你啥就做啥，别人脏活累活扔给你也不反抗，这样他欺负你一次，就有第二次，时间长了就变成人下人了。另一方面是不能被动，不能只知道低头做事情，要用你的长处来展现自己，哪怕只是文档写的好，那也能证明你调理清楚，思路清晰。还要抬头看看路，现在的社会不是一成不变的，是朝令夕改的，你需要识别身边变化的信息和要素，这些都是用来给你做出判断的依据
6. 要多思考，多往前思考
    就这么一句话。
