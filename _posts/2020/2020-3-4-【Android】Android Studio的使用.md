---
layout: post
category: web
title: 【Android】Android Studio的使用
tagline: by 恬静的小魔龙
tag: Other
---

## 一、前言
Android Studio系列文章，主要讲解如何使用这个IDE，原文发布与博客园，请多多支持原作者。

## 二、原文
原文出处： 博客园
原文作者： StephenHe
原文链接： https://www.cnblogs.com/begin1949/p/4966237.html

## 三、正文
### （一）显示行号、快速查找方法源
1、显示行号，只需要右击编辑窗体的边界就可以了。（这种方法只能临时显示，下次打开文件就没了，对其他文件也没用）。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104221352)

2、永久显示行号
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104221409)
3、查找某个变量、类、方法定义的源头，同时可以查找布局文件，资源文件。
> 按住Ctrl键，将光标放到变量的上面，即会出现变量所在的文件，点击就会跳转到相应文件。

### （二）Debug调试
有人说Android 的调试是最坑的，那我只能说是你不会用而已，我可以说Android Studio的调试是我见过最棒的。

好了开始写一个简单的调试程序，我们先来一个for循环

```csharp
for (int i = 0; i < 10; i++) {
//获取当前i的值
    int selector = i;
    //打log查看当前i的值（此步多余，实际开发请忽略）
    Logger.e("for当前的i的值：" + i);
    //调用方法
    stepNext(i);
}
```
设置断点（点击红点位置添加或取消断点）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104238551)
点击debug模式运行
查看调试面板

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104238608)
一、简单调试
1. step over：一步步往下走
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104238648)
 当前程序运行的位置，我们看到i的值已经在程序代码中展示出来了，黄色的代码处，这个是AS的功能，对于我们调试来讲，这简直是非常大的福利了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104238683)
点击单步调试按钮或按快捷键F8，看看效果。这里我们看到selector变量的值已经出来了selector：0,我们在看看黄色位置i的当前值是0。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104238727)
这时我们继续F8，我们切换到logcat查看日志，我打印出的i的值是0，
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104238776)
2. step into：看到方法往里走
比如我们的for循环当中调用了一个stepNext(int i)方法，当我们走到这里想看看这个方法里面的运行过程的时候我们可以这样，当走到这个方法的时候我们可以按下F7,或者如下图的图标。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104238834)
这时就走到了stepNext方法当中。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104238875)
在这里打印了一个log，我们再按一下F8我们来看看Logcat, 这里我打印的log都是为了做教程用，调试我们就不用打log了直接看显示面板就OK了

3. force step into :所有方法看完整
这个是可以看到你所调用的所有方法的实现会让你跟着它走一遍，研究源码使用非常方便
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104238919)
4. step out ：有断点下一个，走完断点继续走
这里如果我们的一个流程当中，包括调用的方法，如果有断点走到下一个断点，如果没有断点，而是在一个调用的方法当中，会跳出这个方法，继续走。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104238972)
这里理解比较难，举个例子：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2018122810423918)
(上图)我现在程序位置在第一个断点位置（24行），我调用的stepNext方法中也有一个断点，此时我按下step out按钮会走到stepNext中的断点处（39行）我此时如果再按一下step out 会走到stepNext方法的调用出的下一个可执行代码（30行）
![在这里插入图片描述](https://img-blog.csdnimg.cn/2018122810423971)
(上图)如果我现在程序位置在stepNext的方法中，如果我此时按下step out，会走到stepNext方法的调用出的下一个可执行代码（30行）

5. run to Cursor ：下个断点我们见
这里的意思就是说，会很快执行到下一个断点的位置，而且可以静如任何调用的方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104239117)
二、高级调试
1. 跨断点调试
如果我们设置了多个断点，现在我们需要直接跳转到下一个断点，那么直接点击下图就可以了
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104239160)
2.观察变量
如果我们想观察1个或者几个变量的值的变化，如果我们在Variables显示面版中观察如果我这里有太多太多的自定义变量和系统变量了，那么就难观察了，我们可以做如下操作：
点击Watches,点击＋号，然后输入变量的名称回车就OK了，而且会有历史记录哦
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104239198)
如果变量名比较长我们可以这样：
选择［Variables］中的变量名然后点击［右键］，选择［Add to Watches],然后Watches面板中就有了
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104239268)
3.设置变量的值
在程序中有很多的条件语句和循环语句，调试也是比较耗时的，我们可以通过快速设置变量的值来加快调试速度，我们可以做如下操作：
选择［Variables］中的变量名然后点击［右键］，选择［Set Value…]或者选择之后直接F2(如上图)（下图为Variables面板）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104239323)
4.查看断点
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104239367)
点击之后我们可以看到所有的断点，以及位置代码,也可以设置一些属性
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104239407)
5.停止调试
要注意的是这里的［停止调试］不是让程序停止，而是跳过所有调试

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104239447)
到这里我们的Android Studio的断点调试和高级调试就完毕了。

### （三）包不分级、修改包名
1、如果不喜欢将包逐级展开的话，可以将每一个包名都完整展现出来，只需要勾选Flatten Packages。![在这里插入图片描述](https://img-blog.csdnimg.cn/2018122810420738)
2、修改包名
![在这里插入图片描述](https://img-blog.csdnimg.cn/2018122810420791)
3、填写新的包名
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104207142)
4、点击确认
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104207189)
### （四）生成Get、Set方法
如何快速生成Get、Set方法，在我们编程中经常使用，下面将详细介绍。

1、右击代码编辑区域，并选择Generate。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104515419)
2、在弹出框中选择Getter and Setter。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104515481)
3、在弹出框中全选所有变量，确定即可快速添加Get和Set方法。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104515531)
4、同时，在Generate中，我们可以快速添加构造方法、toString()、equals()方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104515641)
### （五）导入第三方Jar包
本篇博文将介绍一下如何导入第三方Jar包。

1、首先将下载的Jar包直接Copy到libs目录下面，然后右击Jar文件，点击Add As Library后，在出现的弹出框点击确定即可。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104811683)
2、打开build.gradle文件，可以看到 compile files(‘libs/litepal-1.3.0.jar’) 代码。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104811739)
3、在/.idea/libraries下面会生成相应的配置文件。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104811798)
4、另外你可以通过Project Structure添加

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104811859)
5、添加
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104811899)
6、参考博文：http://blog.sina.com.cn/s/blog_693301190102v6au.html

### （六）如何更新Android Studio

1、导航栏的Help下拉框可以找到更新的按钮。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104943132)
2、接下来点击Update and Restart即可
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228104943192)
3、有些人说网连不上，但我的就可以，不行的话大家就多试几次。

### （七）Structure窗口
1、本篇博文介绍Android查看.Java文件中所有属性和类方法的工具：Structure窗口

2、我们知道Eclipse的OutLine窗口可以查看.java文件所有的属性和方法。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228105045947)
2、Android Studio也存在这样一个窗口，就是Structure（刚安装的Android Studio，该窗口存在最左边，可以移到最右边）。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228105045999)
3、可以通过最上面控件进行相应筛选
![在这里插入图片描述](https://img-blog.csdnimg.cn/2018122810504658)
### （八） TODO代码
我们都知道Eclipse存在// TODO代码，该段代码在方法中用于标识该方法仍未完成，也可以用于作为该方法的一个快捷键。例如我们可以用于标识onClick()方法，当我们需要查找onClick()方法时，可以直接点击编辑区域最右边的标识。

1、Eclipse的// TODO代码
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228105157176)
2、Android Studio的// TODO代码
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228105157229)
3、另外我们可以在最下面查看项目、文件中的// TODO数量和位置
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228105157280)

### （九）设置IDE编码格式
1、打开设置
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228105347226)
2、勾选编码格式，在这里可以设置分别设置IDE、Project、File等级别的编码格式。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228105347275)
3、查看、修改各个文件的编码
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228105347354)
4、当右击编辑界面时，可以直接设置当前文件的编码
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228105347408)

### （十）读取assets、Raw文件夹下文件，以及menu、drawable文件夹
1、直接在/src/main目录下面新建assets目录
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228105512879)
2、接下来即可读取文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228105512933)
3、读取Raw文件夹下文件也类似。首先在res文件夹下新建raw目录，然后放入需要的文件即可读取。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181228105512999)
4、menu和drawable文件和Eclipse位置类似，只需要在/src/main/res文件夹新建就可以了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2018122810551368)

### （十一）每次打开时选择项目，而不是直接进入上次项目
1、打开的时候选择打开哪一个项目
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229095108703)
2、需要在设置System Setting，不要勾选Reopen last project on startup项。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229095108759)
### （十二）怎样打包项目
1、在导航栏的Build下面找到Generate Signed APK…，进入该菜单栏即可。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229095135395)
2、生成Key Store安全钥匙和证书的管理工具。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229095135464)
3、如果已经有了Key Store证书，则可以直接跳过第二步，直接填写Key Store的信息。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229095135517)
4、选择APK文件存放路径，Build类型即可。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229095135572)

### （十三）设置方法分割线
1、只需要在设置中选中show method separators 即可
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229095159901)
### （十四）如何查看资源或者函数在哪些类中被引用
1、我们都知道在Eclipse中可以通过快捷键Ctrl+Shift+G开快速搜索方法、类、资源都在那个类中被使用了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229095220691)
2、在Android Studio中则使用快捷键Ctrl+G。


![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229095220749)

