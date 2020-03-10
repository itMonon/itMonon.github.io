---
layout: post
category: Unity3D-Game
title: 【Unity3D开发小游戏】 《纸箱战争》Unity开发教程
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
游戏玩法类似与战地系列的FPS对抗游戏，玩家可以利用在地图中的载具对目标点发起进攻，最后夺得目标争夺点取得游戏的胜利。

完全原创肯定就不能使用网络上的美术资源和他人分享的代码亦或是插件，鉴于美术基础太差，因此选择了简约的美术风格，有点类似《我的世界》那种方盒子感觉。

## 二、效果图
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY0NHc0azRrMmtnOWdvMzJ5OGcuanBn?x-oss-process=image/format,png)

## 三、正文
开发日记：Unity完全自制游戏《纸箱战争》项目记录 
![这里写图片描述](https://img-blog.csdn.net/20180723165738633?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
## 制作时间为期三周
## 第一天
以下是已完成的部分
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY0MnZzMnNzZWFnYWZ3eXN5ZWYuanBn?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY0M291a3doNXh6eXpwY3V3b3EuanBn?x-oss-process=image/format,png)
因为游戏使用了简约的美术风格，资源占用量大大减少，所以便可以实现次时代大作中难以实现的部分，比如完全的场景可破坏性。

在上图中可以看到，树木可以分部位击落，落在地面后一段时间沉入地面随后销毁物体。.
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY0M3I1Yno1bmI1dm1nYWNnYXguanBn?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY0NHgzMm4ycjM0NHp1ZWU1OXMuanBn?x-oss-process=image/format,png)
人物的自定义外观，这里算是投机取巧，本来是想要实现类似“人类一败涂地”中那种可以涂色的系统，苦于不知道怎么实现，于是就在鼠标划过的位置实例化出一连串的彩色物体代替画笔，最后发现每实例化出一个物体就会增加Batches的占用，估计在项目进展后期可能会删掉这个功能，或者演一下。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY0NHc0azRrMmtnOWdvMzJ5OGcuanBn?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY0NGxhYWQzZmZkYXhkZmZ2bWQuanBn?x-oss-process=image/format,png)
人物动画的状态机，看上去有些乱，不过只有这样才能实现任务在待机、行走、奔跑、换弹、开火、以及死亡之间的切换流畅性。

之后是今天的项目进展记录。

考虑到了全物体的可破坏性，那么地形自然也得是可破坏的，而地形恰恰也是其中最难实现的一部分，因为炸弹的爆炸，地面上肯定会出现大大小小的坑洞，那么这些空洞该如何实现呢？

首先我想到了3Dmax中的布尔，查阅了一下资料，Unity中也是可以实现布尔效果的，不过需要实时计算物体的模型网格，还牵扯到了一种叫做SBP的技术，就我目前的能力而言想要实现是不可能的。

所以我就想到使用小块物体的集合替换掉大块物体，随后对他们的刚体施加爆炸力，模拟出炸弹爆炸效果，通过初步的测试，实现了这个效果。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY0NGZsMjIyZnd3MjF3d2xhNXcuanBn?x-oss-process=image/format,png)
首先地形上是一整个完整的地面，在受到爆炸之后用小方块的堆积物替换掉大块的方块，因为大小和位置的原因，在视觉上有一些欺骗性，爆炸效果很不错，在爆炸后删去碎屑方块，能够在地形上留下一个实体的空洞。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY0NWhzZWlldWg2ZTVxaHBuZWUuanBn?x-oss-process=image/format,png)
随后问题就出现了，因为小方块过多的原因，爆炸空洞的小方块被删去了，但还剩下了很多没有波及到的方块，框选起来，编辑器瞬间就卡顿了，如图的绿色密密麻麻的物体都是小方块的碰撞。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY0NWJ4cjFmc2VtY2NqajdybnouanBn?x-oss-process=image/format,png)
爆炸一个坑洞还不够明显，接连爆炸之后卡顿更加明显，当场景中出现了很多小方块时，就算没有给他们添加刚体组件，场景依然很卡顿。

想了很多方法，咨询了老师也没有好的解决方法。

我觉得爆炸空洞周围有很多浪费掉的方块，从方块分裂的思路上看，这不是个最优的分裂方法，于是我重新制作了模型，之后的爆炸达到了以下的效果。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY0NWkzdHE4YWhkODFwdDg5ZjEuanBn?x-oss-process=image/format,png)
框选之后，空闲的小方块明显减少了不少，帧率也明显提高了
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY0Nm1vbTJjaXVud2dvendrencuanBn?x-oss-process=image/format,png)
仅仅达到这样还不够，因为游戏中还会有载具的出现，所以地图肯定不能只有几十米，当地图变大之后，坑洞越来越多，游戏势必会变得越来越卡。

随后我诞生出了修补坑洞的想法，在坑洞产生后，过一段时间遍把坑洞恢复到之前的状态，不过游戏体验肯定是要比留下爆炸产生的洞差一些的，还在纠结该怎么样处理这个问题。
##第二天
今天是周六，依然进行了部分的制作，昨天采用了更为优化的爆炸方块替换效果，但对性能还是有不小的影响，昨天在临走之前听老师说可以使用遮挡剔除尝试一下，今天就研究了研究，经过查阅资料，实现了遮挡剔除的功能。

具体实现步骤是把需要进行遮挡剔除的方块在静态选项中勾选上Occluder Static和Occludee Static，在遮挡剔除面板中进行烘焙即可，使用起来很方便，在使用后就产生了如下的效果。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY0NmUxNmJiNml3dnc2bDc1dWguanBn?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY0Nmh6MDA2bjA2OHFuNnEwbjMucG5n?x-oss-process=image/format,png)
绿色区域为摄像机的可视区域，在可视区域以外的方块都不进行渲染，有效的提高了游戏运行效率。

新的问题随之产生了，因为游戏中需要切换载具，我发现在切换载具的时候，因为摄像机的变动，遮挡剔除似乎失效了，遮挡剔除似乎只能作用在一个摄像机上，这点问题存疑，还没有得到有效的解决办法。

因为地块崩开之后需要在一定的时间内删除，在Unity中即使物体被删除了仍然会在内存中占有一定的资源，想到利用对象池来解决这个问题，经查阅资料后，发现对象池并不适用与我设想中的地形崩坏，具体还得再次的考量考量。

另外还写了一个通用的炸弹脚本，在游戏中所有的炸弹都可以用到，避免了再每个炸弹上单独挂载脚本的繁琐性。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY0N2F1cjg2YzVoeTU2MXVoYWcuanBn?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY0N3pnMGVhcnh6MXpmZjRyZjQuanBn?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY0OGgzd2dseHdxeGZ5MjB4cWIuanBn?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY0OGx6bmE3ZzdlYTd3Z3dsZ2UuanBn?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY0OXVmM3JnMGFxZGdrcDNxZzMuanBn?x-oss-process=image/format,png)
##第四天
经过周日一天懈怠的洗礼，今天又要投入到项目的构建中来，想想18年已经过去一半了，似乎什么目标都没有实现，挺让人压抑的。

本周是项目正式进行的第一天，刚开始就需要解决周六留下的遗留问题。

给老师看了坦克炮击后留下的弹坑效果，如图上所示，箭头指示的是用到了前天写的炸弹脚本后的效果。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY1MG9nNDFlNmJscmxrYmRubXIuanBn?x-oss-process=image/format,png)
炸弹在爆炸之后消耗地形方块的生命值，不断的分裂最后产生爆炸，在地形上留下一个空洞。

当我操纵人物进入深坑的时候，发现了一个不严重，但足以毁灭游戏的问题。

让操控角色以一定速度碰撞到任何碰撞器之后便会失去移动能力，就像是被卡主了一样，左右移动之后可以脱离，这个问题的出现使得不能从深坑中跳跃出来。

研究了半天，也没有得出个所以然来，咨询了老师得到建议是使用角色控制器来代替本身用于检测碰撞用的胶囊碰撞器，虽然问题可以得到解决，但在之后的对战设计中会存在更大的问题，只能从胶囊碰撞器入手解决。

在网上查阅了相关资料，似乎并没有找到类似的事件的解决办法，大致浏览了一遍Unity提供的第一人称控制器的代码，并没有发现有什么异常的地方。

在之前的测试中，把人体的刚体设置成了正常人的体重用来模拟碰撞效果，把质量设置为1后便没有卡住的问题了，这点还需要在之后进行详细的研究。

从项目确立的时候就有两个可以预见的难度摆在眼前，第一是可破坏物的设置，第二就是敌人的AI，我处理问题喜欢从困难的入手，如果困难的问题都能顺利解决，那么简单的问题就不再算作问题了。

可破坏物的问题算是解决了七七八八，接下来就是另一大难点敌人的AI。

经查阅资料，传统意义上的FPS的AI是要比RTS游戏简单不少的，只要打得准反应快就行了，可我觉得，如果单单是这样，玩家的游戏体验将会大大折扣。

而且传统网游中的AI智商都是相当低下，他们通过寻路系统进攻目标点或者是敌人，没有任何变数，有时候玩家只需要卡一个视野就能如同切菜一般杀死敌人，这样的游戏还有什么意义，还不如不止一个靶场来让玩家练习枪法来的实在。

因为游戏中还存在着可破坏地形以及可破坏物的限制，Unity中的寻路系统显得有些无力起来，在早期测试中，已经烘焙好的寻路网格，就算地面上出现了大型的坑洞，受导航单位走在上面如履平地，当玩家在游戏中游玩看到敌人能够在坑洞上行走，显然是很不符合逻辑的。

没办法，只能自己解决AI的寻路问题。

在我的设想中，应该是AI会自行选择目标点，随后朝着目标点发起攻击，期间伴随着一些突发事件，诸如驾驶载具等等...

最早我是打算通过一个AI脚本就实现全部的AI功能，随后发现这会有一定的局限性，而且就算是同阵营的NPC之间也很难产生一定的联系，在游戏中会产生诸多缺点，诸如全部的NPC都进攻同一目标点的行为。

于是我想到RTS游戏，在RTS中，玩家扮演的一般是能纵观全局的领袖，指挥手下的士兵也只是给一个大致的目标点，具体的开火和战斗还是由AI智能实现的。

通过这点，我想到把AI分为两个模块，其中较大的作为中央控制AI，负责对战局进行分析，哪些是需要进攻的目标点，哪些是需要防守的目标点，派遣多少士兵对这个据点进行防守等等。

而较小的AI脚本则挂载在NPC控制的士兵身上，他们在得到具体的目标之后则会对目标发起进攻，当任务完成之后会向中央控制AI返回“进攻成功”的指令，中央控制AI随后会发布新的命令给士兵AI。

在着手进行AI代码编写之前，首先我创建了一个新的工程对AI进行预测试，就像当时测试可破坏地形一样，这样即使修改了某些东西导致Unity工程损坏也影响不到已有的工程文件。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY1MHZ1N3BpOWFhcHA1d3FjYTIuanBn?x-oss-process=image/format,png)

如图所示，地面上有两个阵营的士兵各一个，还有四个目标点，其中A点为红色方占有，B点为蓝色方占有。

再进行AI编写前还需要先能让AI知道什么时候该进攻目标点，什么时候该防守目标点。

这里我把目标点设想为一个环形区域，只要在此区域内的阵营物体都可以参与目标点的争夺。

##理论如下：

当目标点为红色方所有，蓝色士兵进入区域，对目标点执行占领，需要先消耗掉目前红方的已有占领条，一个单位即记为消耗速度为1，多个单位则进行累加，最高上限速度为3。

当占领条消耗完毕，目标区域则变为蓝色，同时对占领条进行增加，一个单位记为增加速度1，多个单位累加，最高上限速度为3。

当占领条重新增加到满为止，这个据点才算被成功拿下，可以作为重生点使用。

如果在占领过程中同时有敌方士兵进行防守，每个敌方士兵会减少一点的占领速度，如果双方在占领圈中人数相同，则占领区域既不增加也不减少。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY1MHlqMTU3aTE0cTF2dXo3MXEuanBn?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY1MWFtdndsYmkxNmRpbmx3ZGMuanBn?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY1MXAxaDEweGgyY3ZqaDEwOXMuanBn?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY1Mm03M3JycTdkM2lpajVxOHUuanBn?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY1M2FqcTFqcTRxeThidnN5amIuanBn?x-oss-process=image/format,png)
在代码中，我定义了一个函数叫“目标滑向阵营”，不知道该怎么形容，大概意思就是判断该区域内成员较多的一方势力，这就说明目标点的占领方向是向着该方向倾斜，主要用来修正当占领条不满时，据点内双方都不存在人数的情况。

说句实在话，今天的状态实在是不好，可能是因昨晚没有休息好缘故，在处理逻辑的时候总感觉意识有点模糊似的。

代码写完之后，感觉像是写错了，Debug一下居然输出了正确结果，也让我自己有点小意外。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY1NGhlZzJzYTQ2NGo0amF6bWcucG5n?x-oss-process=image/format,png)
- Point Rad为目标点的范围；
- Point Camp为目标点的阵营编号；
- Point Want Camp为目标点滑向阵营；
- Point Value为目标点的占领值；
- Occupy Number为占领区域内的总人数；
- Occupy Speed为占领的速度。
这样是在面板中可以定义的属性，在测试阶段看上去数目繁杂，有些很乱，当测试通过之后，就可以把不需要外部调用的变量从公有改成私有，虽然不知道这么做还有什么好处，最起码在设置参数的时候看上去能舒服一点。

完成了目标点的占领测试之后，项目近战又前进了一步。

接下来是AI总控制器和个体AI的编写，因为两者之间存在着很多练习，所以在这个程序中，我选择同时编写两个AI。

在编写个体AI的时候，我想到战争时期，通讯不便的情况下是如何发布战斗指令的，从烽火狼烟联想到飞鸽传书。

两者都是用来传递信号的，但都有一个通病，传送的信息量太小，飞鸽传说或许还能好一些，狼烟兴许能通过改变烟雾的节奏？这个说不清楚。

接着还联想到了Unity中的动画状态机，似乎动画状态机的运作方式跟我设想的很相似，已经既定好了若干个行动目标，如果达成了某项条件，就执行预先设定好的动作。

照着这个思想，我想到了AI总控制器给个体AI传递信息的时候可以只传递坐标信息，动作制订则由预先编写好的方法体来实现，通过代号来判断具体指令的目的。

比如“0000”是朝向目标点行进，“0001”是朝着目标点行进，在目标点中环形运动巡逻等等...

在AI总控制器中我也把目标点的可重生性和可攻击性以及攻击的先后顺序进行的方法预编辑，在使用的时候方便调用。

预计整个AI总控制写下来在两千行左右，同时还要处理大量的逻辑问题，对我这样一个初学者来说算是个不小的挑战。
##第五天
今天因为公司来了一支拍摄团队来拍摄宣传片的缘故，项目中断了几次，还客串了一把演员出镜了一段，对项目进度产生了些许影响。

同老师讨论了一下昨天构想的两种AI设计思路；

第一种是从RTS游戏入手，用控制AI包裹住个体AI，呈向内辐射的控制模式，个体AI返回下达指令的完成情况，控制AI结合战场局势修改指令的AI设计模式。

第二种则是摒弃控制AI，把所有的个体AI孤立成为类似动画状态机的条件实现模式，在满足一系列条件之后根据与非逻辑进行判断操作。

因为之前完全没有接触到AI领域，以片面的权衡考虑来说，第二种的AI似乎更容易实现一些，但整体效果似乎与第一种相差很多。

如果按照第二种AI逻辑实现的话，项目中的NPC将不会给到玩家一种战争的感觉，更像是过家家的打闹一般。

从老师那里得到的印证也比较倾向于用第一种设计模式来实现NPC的AI效果。

在预先构想的同时觉得是很容易实现的事情，当着手操作的时候并没有想象中的这么简单；

人脑是相当复杂的结构，由数以亿万计的神经元细胞连接而成，神经网络错综复杂，而计算机中只存在01的判定，所谓AI在我理解中也应该就是若干个判断语句链接在一起构成的。

一条if语句其实就可以称作一个简易的AI了，但如果想让这个AI聪明起来就需要更多的if语句，if语句累积起来就可以凑成相当大的神经网络系统。

举个例子，就像是吃早餐：

面前摆放有一个包子，如果只有一条if语句的话，就只能判断对这个包子是吃亦或是不吃，那么条件也肯定就是自身的饥饿程度。

对吃包子再加上一条if语句，比如像是包子的馅料，如果我是个素食主义者，而包子是肉馅的，及时我已经饿了，但我也不会吃这个包子。

在此基础上再加上一条if语句，如果我现在已经快要饿死了，那素食主义者肯定就没有需要再坚持的必要了。

就像这样，三条简单的if语句就构成了一个简易的逻辑网格，那么更多的逻辑网格拼凑在一起岂不是就出现了类似人类的思想？

从早上起床的时间，选择穿着的服装，早餐吃的什么，乘坐怎么样的交通工具，这些够可以使用一连串的条件来判断结果。

有时候未免会出现模棱两可的情况，似乎进入某个选项都会有冲突，人类都可以通过抛硬币来结局，计算机自然也是可以通过随机数来解决的。

当然了，这只是我作为AI尚未入门者的片面认识，如果有理解错误的地方还希望能够斧正。

扯得有些远了，不过这也能够佐证一个尚可AI的书写难度。

为了保证代码的易读性，我选择把控制AI中的所有复杂算法全部封装成了方法，目前还只是封装了几个，如图所示：
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY1NHhkNGhnN2g3bmQycDIxMWMuanBn?x-oss-process=image/format,png)
首先控制AI要知道自己是属于哪个阵营的，随后需要知道各个目标点现有的占领情况，这些都是不可避免的，在游戏的初始化中就调用了该方法。

随后我想到在玩过的战争FPS游戏中，玩家普遍都是会分有小队的，指挥官会分小队下达命令，如果要一个士兵一个士兵的下达命令将会显得指令赘余，所以在游戏刚开始的阶段，就根据设定的阵营士兵数量对AI分成小队。

这里我不想再设定更复杂的多种小队了，直接让阵营中不管士兵数量都分为ABC三个小队，整除一下，剩余的士兵再统一划拨给A小队。

既然已经给士兵区分了小队，那么在士兵身上挂载的仅用于判断属性用的脚本上就要增加上一个小队选项，这样指挥官就可以通过不同的小队下达进攻亦或是防守指令。

在之前的设定中，目标点占领只有在值达到100的时候才可以使用该目标点重生。

这点也同样封装进了方法中：
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY1NGZrdGV4NWVucDJrNGUybnguanBn?x-oss-process=image/format,png)
在之前的代码中除了变量声明带有注释外，算法部分都是没有注释的，应该不影响理解。

该方法是返回一个初始化小队实例化完成的bool值。

首先执行的RebornPointOK（）方法是判断目标点是否允许重生。

如果A点和B点都为可重生状态则进入之后的函数，根据小队成员的现有数量判断小队是否满了，不然就给该小队实例化一名成员。

其中的Camp值为最先声明好的控制AI的阵营，1为红色，2为蓝色。

最后本日的项目进展就到此为止了，此外还封装了一个判断目标点进攻优先级的顺序，算法还不是太成熟，就先不展示出来了。
##第六天
今天设计完成了初代版本的AI制作，先命名为AI1.0。

在AI1.0中，初步实现了控制AI程序对个体AI发布指令的功能，随着时间的发展和目标点占领状态的不同，管理AI会发布不同的AI指令给个体AI，个体AI则会遵照指令行动，经过一段时间的测试，初步达成了既定的目标，对目前的设计还算满意。

在早期版本中，因为个体AI挂载在模拟士兵单位的方块上，方块带有碰撞和刚体，运动显得有些不正常，锁定了刚体的轴向限制后就产生了图二中的效果。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY1NWhiYXphd2d3ZXpxZHR6aHEuanBn?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY1NXpidDV2eDUweTl5emw5Z3kucG5n?x-oss-process=image/format,png)
另外，在测试中发现了一个控制点的Bug，不知道为什么在昨天的测试中没有发现，检查代码后发现在设计控制点算法的时候，考虑到了在控制点周围的如果没有士兵的情况，为了节省代码的运算量，算法被设计成控制圈内物体数量发生变化时才进行后续的计算。

这样的话就出现了一个问题，在昨天的设计中提到了一个“目标滑向阵营”函数，如果目标点中的人数始终不发生变化的话，那么“目标滑向阵营”函数就不能重新赋值。

所以目标点的占领数值在降到0之后会错误的把目标点的阵营规划到“目标滑向阵营”的数值，在改变算法之后，把整体算法封装成一个方法，使用重复计数器重复调用函数，这样也能做到节省程序计算步骤，同时又可以对“目标滑向阵营”进行赋值。
##第七天
今天完成了初代可试玩版本的发布，历经将近一周的时间的首个测试版本。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY1NWV0YnVxcWI2NHI2NHpzaWouanBn?x-oss-process=image/format,png)
初代地图并不掺杂地形，只不过是把外建工程中的AI脚本适配进了原项目工程中，对细节部分做了一些优化和改动。

在占领点上增加的旗帜，方便更直观的看到旗帜的变化从而知晓占领点的占领进度，当然了，这在之后的UI上也会有所体现。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY1Nm55YWpxb3gzM3lycmp6Z2wuanBn?x-oss-process=image/format,png)
如图所示，不同占领点在AI接近之后发生了占领数值的变化，从而改变了旗帜的高低位置。

在旗帜的代码编写阶段还出现了一点意外情况，感觉到利用动画实现旗帜的升降会更加困难，所以便采用了代码检测目标点的数值动态调整旗帜高地的设定。

在最早的代码中，因为错误的把全局坐标当成了局部坐标，创建向量给position赋值的时候坐标位置总是不再预想中的位置上，期间还以为是因为模型的缩放值影响到了坐标位置的变化，最后检查过发现是在赋值的时候发生的错误，创建出的向量为全局坐标向量，但错误的赋值给了局部坐标，从而导致了错误，更正之后即修正了错误。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY1Nmp3NXZrZXF3d3p3enRjNXEuanBn?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY1NmQxZXJtZmd1MWZnMHp5cmYuanBn?x-oss-process=image/format,png)
上图代码中是实现的NPC在死亡后所调用的函数，在NPC生命值小于0的同时关闭物体的显示，同时在OnDisable中调用管理AI中的重生方法，传导启用协同程序，在等待一段时间后根据随机值在可重生点复活。

接下来的就是着重与游戏的优化，因为地形破坏的原因要消耗大量的系统资源，所以在其余能节省的地方要尽量节省，首先我考虑到了管理AI的代码深度过大，每次执行都要消耗扥多资源。

而在战争进行的同时，目标点肯定不会是瞬息万变的，所以统一调用时间设置成了五秒，每五秒钟更新一次目标点，饶是这样还感觉有点时间短了，在后续添加进地形之后可能会把该数值放大到10秒或是20。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY1N2gweG96bGw1aml3djJsaXguanBn?x-oss-process=image/format,png)
如图产生爆炸效果的同时，工程页面的FPS值为82，因为我用到了两个屏幕的原因，消耗的性能可能会增加，如果是在玩家的电脑上运行帧率应该会进一步提高。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY1N202a2E0N2U5bXZ2Z3owYTkuanBn?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY1N3hpMjF1dDZjOTAxdDY2MzUucG5n?x-oss-process=image/format,png)
驾驶飞机时的瞬时FPS达到了187，多数还是能维持在120。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY1OGJqcTA5dnpua2YyOWt2dmsuanBn?x-oss-process=image/format,png)
飞机拉升到纵览全局的高度，地形全部加载，预先设定好的四个目标点区域在地图中只占到了很小的一部分，现在的FPS为105。

最后增加了算是一点恶趣味的东西，通常在这类游戏中，玩家进攻的目标点一般都是不可摧毁的，也不能改变的，但我给目标点旗帜增加了刚体，当地形破坏之后，旗帜就掉进了地面下的孔洞中，当然了使用坦克也可以推动旗帜。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY1OGJuamxpcmFsYWF0bHUyMHcuanBn?x-oss-process=image/format,png)
此外，今天在编写代码和工程页面调试阶段Unity发生了多次崩溃，未能发现崩溃的具体原因，把工程发布成产品之后在别的电脑上均能流畅运行，并无报错闪退的情况，目前还不清楚是因为什么导致了这样的结果，难道是因为在编写代码的时候改变了系统的某些环境变量的缘故？
##第八天
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY1OGZnOWIyc21tbW50OWpuOXcuanBn?x-oss-process=image/format,png)
今天完成了一些场景模型的布置搭建工作，还在发愁项目优化的问题。

AI的总体控制完成了，但个体AI离真正的实现还有很长的一段路要走，今天特意抽出了一些时间来思考个体AI的设定。

首先可以肯定的是，个体AI肯定是要比控制AI更加复杂多变。

其次，今天突然之间想到，如果是用AI来模拟真实的玩家，那么真正的玩家在游戏中的行为肯定会是无法预测的，并不会按照某一固定的模式执行规则。

因此之前设想过的通过需先编辑好的AI动作方法就只能放弃了，重新整理思路。

一定要确定的是一点，游戏中是分有不同的职业的，现阶段是分为突击兵、工程兵、反载具兵、医疗兵、狙击手。

在敌人是医疗兵或者是狙击手的情况下并不能对敌人的载具造成伤害，那么载具肯定就不会是他们的目标。

而工程兵或者是反载具兵的优先攻击目标肯定是威胁更大的地方载具单位。

医疗兵则会优先治疗友军，其次才是击杀敌人，狙击手则是要远程击杀目标。

考虑到这点，我就思索是不是应该把个体AI的寻获目标方式给封装成独立方法，因为并不需要时刻切换目标，和控制AI脚本一样，每隔一段时间调用一次就可以了。

在确定敌方目标的时候，我使用到之前写过的Camp脚本上的阵营，这样一来就需要多次的获取组件，担心多次的获取组件会影响到游戏的性能，对本就高负荷的游戏再增加更多的负担。

查阅资料后，发现获取泛型组件是相对节省资源的方法，但尽是这样还不够，考虑到了在遍历循环中每次遍历判断的时候都要获取几次组件，干脆把获取组件集体构造一次，这样应该能节省一些资源。

在获取了目标之后，就应该是对目标发起攻击了，这点考虑到了还有进攻目标点的原因，我认为应该添加一个追击时间，在追击了一段时间后，AI则会放弃追击，进攻目标点。

到目前位置，没有框架的致命缺陷就暴露了出来。

自我反思，在项目确立的时候就没有构思好全部的步骤，有些功能的实现只是因为一时的兴起而制作了，这样反而是打乱了整体的设计路线，如果有一个成熟的框架，代码编写则会简单很多。

早在项目建立阶段就没有考虑到这个问题，对自己的能力认识的还不够清晰，同学制作的手游几近完成，而我还卡在敌人的AI设定方面。

不过这也并没有打击到我的自信心，毕竟是PC游戏，复杂性远非那些手游所能比拟的，这对我是一次锻炼，也是对前段时间学习的一个总结。
##第九天
其实周六也是项目有些进展的，不过是在因为进展太少了，不值得单独写成一篇记录。

这两天主要都是在整合AI的功能实现，不得不说，写AI实在是太麻烦了，比想象中的还要麻烦的多得多。

今天实现了AI对目标发起攻击的算法设计，本来打算是要自己写出来一套躲避障碍物的算法，实在太难了，没办法不得不采用了Unity中自带的寻路导航系统。

如下图片是打印出了AI的设计检测射线和路径导航的预览图。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY1OW1wdTMxbzFtcmx2cWpqdmkuanBn?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY1OWNyaWd1em41aWRuNTRvemUuanBn?x-oss-process=image/format,png)
今天上午写了删删了又写，花了一上午的时间算是写出了一个AI目标判定的方法。

之前已经书写过了AI搜寻目标的方法TargetFinding（），在使用的时候只需要调用就行了，这就不需要在AI脚本中获取了。

我考虑到在AI运动的时候应该只有四个状态：

1. AI没有攻击目标，AI处于占领区域内

2. AI有攻击目标，AI处于占领区域内

3. AI没有攻击目标，AI没有处于占领区域内

4. AI有进攻目标，AI没有处于占领区域内

如果为3的情况，只需要让AI向着目标前进就行了，同时再路径行动的过程中搜索可以攻击的目标。

如果为4的情况，AI找到了攻击目标，但没有处在占领点内，这时候就需要让AI对目标发起攻击，但并不能耽误了进攻目标点。

如果为2的情况，就直接让AI进攻目标，并且在杀死了目标之后立即寻获目标。

如果为1的情况，就让AI呈现巡逻状态，始终处于寻获目标的状态，直到发现敌人。

以下代码为AI的职业为突击手时的AI算法：
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDY1OXN1YWVoMWFuaWIxanN6OHAuanBn?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDcwMHRvOXhsdDVleGN1bzlvZnUuanBn?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDcwMGk3OWRmbzJkd2h6YXd6aHouanBn?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDcwMXpxdHBkc3NxMXM4N244aTEuanBn?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDcwMWwxb2Y5em85Ym01ODMzOTMuanBn?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDcwMmQ3MnI3cnhrcjJ4aXdrNWkuanBn?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDcwMndpZnpjeGprODVyNXI4a3guanBn?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDcwM3JnaDM5aG83MzRnZzAwMmYuanBn?x-oss-process=image/format,png)
##第十天
今天整理了个体AI的脚本，也算封装完成了
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDcwM29sNHNlcTNtcW50bTJ1dGIuanBn?x-oss-process=image/format,png)
封装以后，原先预计在2000到3000行的代码被精简到了700行以内。

经过测试，流畅是很流畅，但出了一个很严重的BUG。

不清楚是具体什么原因，AI总是会攻击队友，而且次数不在少数，莫不清楚头绪，检查了几遍自己封装的方法，初步估计原因是在目标寻获方法中出现的问题。

找来找去终于发现是因为一个简单的逻辑运算符给打错了，没想到居然会造成了这么严重的BUG，看来码代码的时候还需要更加仔细一些。

此外今天还解决了报错闪退的原因，闪退前控制台会瞬间报错后闪退，今天手快截图到了，在网上查了下解决方法。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDcwNHFiNWN5cW1rYmNycnJka2YucG5n?x-oss-process=image/format,png)
我使用的Unity为5.45，在国内的网站上居然找不到解决方法，最后还是在外网上找到了类似的解决方案。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RpLmdhbWVyZXMuY29tL2F0dGFjaG1lbnQvZm9ydW0vMjAxODA3LzE2LzEzNDcwNHVraWJta3ZreWs2cXE0eWMuanBn?x-oss-process=image/format,png)
大概意思是说出现这个错误的原因是因为使用到了遮挡剔除的原因，相机上勾选了遮挡剔除，而场景中并没有对遮挡剔除的物体进行烘焙，才导致了这样的错误。

我试验了一下把已经红烘焙过但是没有使用到的遮挡剔除清除之后就再也没有出现闪退的情况。

之前还一直认为是因为用到了随机数Random的原因，因为Unity和C#中各有一个Random类，如果没有具体声明可能就出现错误，我还特意加上了Unity的命名空间来解决，没想到居然是因为遮挡剔除的原因才闪退的。

既然这样了，或许在之后的任务中就不能使用到遮挡剔除来优化游戏了，这么说还需要再别的地方想办法节约一定的资源占用量，又是不小的工程累积。

明天预计会完成AI驾驶坦克和飞机的工作，如果时间充裕的话还会设置到玩家控制角色的各种设置。

未完待续。
