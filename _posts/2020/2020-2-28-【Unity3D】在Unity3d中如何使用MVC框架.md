---
layout: post
category: Unity3D-Daily
title: 【Unity3D】在Unity3d中如何使用MVC框架
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
MVC是一种比较常见的设计框架，是一种将代码的功能进行划分的一种设计框架，主要原理就是将软件用户界面和业务逻辑分离，以使代码可扩展性、可复用性、可维护性、灵活性加强。
MVC在桌面应用程序，以及网页架构上面用的比较多，那么怎么应用到Unity3d中呢，下面就带大家去了解这个设计框架，以及如何在Unity中应用。
## 二、MVC介绍
### 简介
MVC全名是Model View Controller，是模型（Model）-视图（View）-控制器（Controller）的缩写，一种软件设计典范，用一种业务逻辑、数据、界面显示分离的方法组织代码，将业务逻辑聚集到一个部件里面，在改进和个性化定制界面及用户交互的同时，不需要重新编写业务逻辑。MVC被独特的发展起来用于映射传统的输入、处理和输出功能在一个逻辑的图形化用户界面的结构中。

**Model（模型）** 是应用程序中用于处理应用程序数据逻辑的部分。
　　通常模型对象负责在数据库中存取数据。
**View（视图）** 是应用程序中处理数据显示的部分。
　　通常视图是依据模型数据创建的。
**Controller（控制器）** 是应用程序中处理用户交互的部分。
　　通常控制器负责从视图读取数据，控制用户输入，并向模型发送数据。
　　
MVC开始是存在于桌面程序中的，M是指业务模型，V是指用户界面，C则是控制器，使用MVC的目的是将M和V的实现代码分离，从而使同一个程序可以使用不同的表现形式。比如一批统计数据可以分别用柱状图、饼图来表示。C存在的目的则是确保M和V的同步，一旦M改变，V应该同步更新。 

模型－视图－控制器（MVC）是Xerox PARC在二十世纪八十年代为编程语言Smalltalk－80发明的一种软件设计模式，已被广泛使用。后来被推荐为Oracle旗下Sun公司Java EE平台的设计模式，并且受到越来越多的使用ColdFusion和PHP的开发者的欢迎。模型－视图－控制器模式是一个有用的工具箱，它有很多好处，但也有一些缺点。
### MVC的业务流程
![在这里插入图片描述](https://upload-images.jianshu.io/upload_images/872318-ab743552bb7f3045.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/579/format/webp)
### MVC是设计模式吗
MVC是一种设计模式，但是却不在Gof总结过的23种设计模式中，所以确切说MVC不是一个设计模式，而是多种设计模式的组合，而不仅仅只是一个设计模式。
组成MVC的三个模式分别是组合模式、策咯模式、观察者模式，MVC在软件开发中发挥的威力，最终离不开这三个模式的默契配合。 那些崇尚设计模式无用论的程序员，请了解只要你们使用MVC，就离不开设计模式。 
组合模式只在视图层活动， 视图层的实现用的就是组合模式，当然，这里指的实现是底层的实现，是由编程框架厂商做的事情，用不着普通程序员插手。



组合模式的类层次结构是树状的， 而我们做Web时视图层是html页面，html的结构不正是树状的吗，这其实就是一个组合模式的应用，只是浏览器厂商已经把界面相关的工作帮我们做掉了，但它确确实实是我们应用MVC的其中一部分，只是我们感觉不到罢了，这也是我们觉得View是实现起来最简单最没有歧义的一层的原因。



除网页以外的其他用户界面程序，如WPF、Android、ASP.NET等等都是使用树状结构来组织界面控件对象的，因为组合模式就是从界面设计的通用解决方案总提炼出来的。所以与其说MVC选择了组合模式，还不如说组合模式是必定会存在MVC中的，因为只要涉及到用户界面，组合模式就必定存。事实上即使不理解组合模式，也不影响程序员正确的使用MVC，组合模式本就存在于程序员接触不到的位置。 



然而，观察者模式和策略模式就显得比较重要，是实实在在MVC中接触的到的部分。



观察者模式有两部分组成，被观察的对象和观察者，观察者也被称为监听者。对应到MVC中，Model是被观察的对象，View是观察者，Model层一旦发生变化，View层即被通知更新。View层和Model层互相之间是持有引用的。 我们在开发Web MVC程序时，因为视图层的html和Model层的业务逻辑之间隔了一个http，所以不能显示的进行关联，但是他们观察者和收听者的关系却没有改变。当View通过http提交数据给服务器，服务器上的Model接受到数据执行某些操作，再通过http响应将结果回送给View，View（浏览器）接受到数据更新界面，这不正是一个接受到通知并执行更新的行为吗，是观察者模式的另一种表现形式。



但是，脱离Web，当通过代码去纯粹的表示一个MVC结构的时候，View和Model间无疑是观察者和被观察的关系，是以观察者模式为理论基础的。即使在Web中因为http壁垒的原因导致真正的实现有点走样，但是原理核心和思路哲学却是不变的。



最后是策略模式。策略模式是View和Controller之间的关系，Controller是View的一个策略，Controller对于View是可替换的， View和Controller的关系是一对多，在实际的开发场景中，也经常会碰到一个View被多个Controller引用，这即使策咯模式的一种体现，只是不那么直观而已。



总结一下，关于MVC各层之间关系所对应的设计模式



View层，单独实现了组合模式

Model层和View层，实现了观察者模式

View层和Controller层，实现了策咯模式



MVC就是将这三个设计模式在一起用了，将这三个设计模式弄明白，MVC将毫无神秘感可言。如果不了解这三个设计模式去学习MVC，那不管怎么学总归是一知半解，用的时候也难免不会出想问题。

## 三、参考文章
深入理解MVC https://blog.csdn.net/zuiyingong6567/article/details/80150834
MVC简介 https://www.cnblogs.com/diyunfei/p/6752618.html
【Unity】MVC框架  https://blog.csdn.net/qq_39574690/article/details/80757261
Unity中MVC简单应用 https://www.jianshu.com/p/acc689be52f9
【Unity3D】手游开发实践《腾讯桌球》客户端开发经验总结 https://blog.csdn.net/q764424567/article/details/81020381
MVC框架 https://baike.baidu.com/item/MVC%E6%A1%86%E6%9E%B6/9241230?fr=aladdin&fromid=85990&fromtitle=MVC

## 四、Unity中使用MVC
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106152450894.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
本篇文章主要是讲一下我是如何在Unity中使用MVC模式，MVC模式肯定不能照搬进Unity中，所以很多东西都做了修改，以适合我的项目使用，如果有不对的地方，希望大家能指正出来。
###  使用原理
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106153441984.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
代码文档结构就是这样的
目前我了解的MVC应用到Unity中只有两种模式，一种就是先按照业务功能划分，再按照MVC划分
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106153620280.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
蛋糕心语游戏就是这种模式，使得模块更聚焦（高内聚）
第二种就是先按照MVC划分，然后再分出来业务功能
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106153441984.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
第二种方式用多了发现随着项目的运营模块增多，没有第一种那么好维护。

MVC的关键就在于业务划分，代码分离，其实跟三层架构比较相似，把业务层、逻辑层、数据层分开来，互相通过一个接口进行调用，但是并不知道接口是怎么实现的。

所以现在的问题就在于如何去划分呢。

实现：
Model（模型层）：
在这一层主要就是存放用户的数据，UI的数据，静态字段，数据存储，以及模型贴图资源的存储
比如说：
 - 存放静态字段
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106154428559.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
- 存放数据
![在这里插入图片描述](https://img-blog.csdnimg.cn/2018110615450465.png)
- 存放模型材质字段
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106154547480.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
- 数据库增删改查操作
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106154637715.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

View（视图层）
在这一层主要是放一些UI参数，获取UI数据，获取按钮事件等
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106154808347.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2018110615481882.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
点击事件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106154911417.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

Controller（控制层）
这一层就是去实现业务逻辑功能，获取Model的数据，通知View层更新数据，承上启下的功能
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106155036737.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

## 五、实例
就带大家实现在Untiy中使用MVC模式，实现更换图片的功能
步骤一：设计界面
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106160146402.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106165111710.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
步骤二：MVC搭建
Model_Main.cs
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106162309375.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
View_Main.cs
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106162348451.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
Controller_Main.cs
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106163538268.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106163551946.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

关于那个鼠标点击事件那个方法，在我另一篇文章中有讲到，不懂的可以去那篇帖子仔细看一下。
https://blog.csdn.net/q764424567/article/details/82424308
效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106163900588.gif)
源代码已经上传到CSDN，按需下载
https://download.csdn.net/download/q764424567/10768387







## 六、总结
PS：程序员最重要的就是总结，不然学的再多到关键时刻用不上还是白搭，总结梳理，然后写下来，当快忘了的时候可以回来看看，会有不一样的体悟哦。
在Unity中应用MVC模式已经讲解完了，MVC模式其实就是多种设计模式的结合体，也是三层架构的改良产品，基本原理就是把业务逻辑、功能实现、数据给分开来，利于增加功能，和维护。
俗话说的好，一千个人就有一千个哈姆雷特，每个人研究MVC之后，运用到Unity中的方法都不尽相同，方法也是很多样，我主要就是提供一个想法和思路，更多优化运用还要大家多开动脑筋咯。
最后，再回顾一下
Model（数据层）：存放静态字段、数据存储、模型资源存储
View（视图层）：就是用户可以看到的层，指的是可以看到的UI，模型，加载和调用事件
Controller（管理层）：实现业务逻辑功能、加载模型资源、功能实现等
