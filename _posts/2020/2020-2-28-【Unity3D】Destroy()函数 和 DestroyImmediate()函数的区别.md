---
layout: post
category: Unity3D-Daily
title: 【Unity3D】Destroy()函数 和 DestroyImmediate()函数的区别
tagline: by 恬静的小魔龙
tag: Unity3D
---

Destroy()函数 和 DestroyImmediate()函数的区别
![在这里插入图片描述](http://mmbiz.qpic.cn/mmbiz/LJ2FKOSh48G1zs5qgU5RKv1pcI6VPMia3gbaGm2SMCcvjp9uwP4BmQQNk07Ce2mMwunuSoICCPJdpNVwMM5q1HQ/0)
目标：
这篇文章的目标是了解Destroy()函数和DestroyImmediate()函数之间的区别。

这两个函数的最终作用是相同的，都是去销毁游戏物体，但唯有一点点小区别，我们去理解这个小区别是非常重要的，错误的使用这些函数可能会导致你的应用层序崩溃。

下面，用一点理论，和测试代码去理解它。

Destroy()：
- 该函数接收一个GameObject类型的参数

- 该函数在当前帧结束后设置传入的GameObject参数对象为Null

语法：Destroy(GameObject);

让我们看一下测试代码：
![在这里插入图片描述](http://mmbiz.qpic.cn/mmbiz/LJ2FKOSh48G1zs5qgU5RKv1pcI6VPMia3TIOPBnQ89GLRiaTPLGkYmcv714hzQbpYsDWQOPAhoVMCk5BcS5F36EA/0)
创建一个新的脚本，并为该脚本添加如上代码，然后把该脚本绑定到场景中的Main Camera上，点击Play进行测试。然后在点击Play后在场景中按下鼠标左键，此时你会发现在Hierarchy面板中多了一个名为myObject的游戏物体，这时按下键盘的空格键去销毁该游戏物体，然后点击console窗口查看Log信息。此时你会看到我们的status打印出了true，然后我们的currObject的名字也被打印出来了。这说明我们的游戏物体在此时（当前帧中）还是存在的。
![在这里插入图片描述](http://mmbiz.qpic.cn/mmbiz/LJ2FKOSh48G1zs5qgU5RKv1pcI6VPMia30rWPwwHTc2FmA3akt2kOMQqzibiamH6JGURPXR2qSiagLI8MPibhVEbBiaw/0)
好了，现在让我们看一下DestroyImmediate()函数

DestroyImmediate()
- 正如它的名字显示所示（Immediate意思为立刻、立即），当GameObject类型参数传递进来时，只要一执行该函数，则该游戏物体就会立刻销毁。

- 该函数会立马销毁物体，不会等到当前帧结束之后才销毁物体

- 语法：DestroyImmediate(GameObject)

现在，让我们在上面的代码中取消DestroyImmediate(currObject)的注释，如下所示：

![在这里插入图片描述](http://mmbiz.qpic.cn/mmbiz/LJ2FKOSh48G1zs5qgU5RKv1pcI6VPMia3090W01SW7LvHzWyYDcZMhRD4AA5zExJO2N95ztsnHcLtUxERo7CWPQ/0)
如上操作，点击Play按钮，按下鼠标左键，然后再按下键盘上的空格键，打开Console窗口查看Log信息。这时你会发现，我们的status变为了false，currObject的名字为Null了，说明游戏物体已经被真正的销毁了。

![在这里插入图片描述](http://mmbiz.qpic.cn/mmbiz/LJ2FKOSh48G1zs5qgU5RKv1pcI6VPMia38icIon2ZbA3t1ibo7CYA3LegaBVu1vKFoJwkGNc07JGyJWKgmfxd9H7w/0)
总结：
Destroy()函数在当前帧结束后才会真正的销毁物体（也就是让GameObject设置为Null），然而DestroyImmediate()函数则是直接在当前帧销毁物体（也就是让GameObject设置为Null）。

要小心使用DestroyImmediate()函数，根据官方文档，你应该尽量避免使用DestroyImmediate。

附上官方中文文档图（Unity圣典的）：
![在这里插入图片描述](http://mmbiz.qpic.cn/mmbiz/LJ2FKOSh48G1zs5qgU5RKv1pcI6VPMia3xR3Z1tDdu5ZugENnekOsDgNnOiahUcLCZW1suPN7vIQhX2VojPFrKpg/0?wx_fmt=png)
Destroy()函数
![在这里插入图片描述](http://mmbiz.qpic.cn/mmbiz/LJ2FKOSh48G1zs5qgU5RKv1pcI6VPMia3CA6ORFEuN1mqZQ5fWGLib45P1PevaeE5ddWBRXKqJnluw9ALOBGbF5Q/0?wx_fmt=png)
DestroyImmediate()函数
