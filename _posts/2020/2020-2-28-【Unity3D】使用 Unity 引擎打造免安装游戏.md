---
layout: post
category: Unity3D-Daily
title: 【Unity3D】使用 Unity 引擎打造免安装游戏
tagline: by 恬静的小魔龙
tag: Unity3D
---

![在这里插入图片描述](https://di.gameres.com/attachment/forum/201903/25/095257ouiz4y6w10v0zymp.jpg)
自从免安装游戏(Google Play Instant)于2018年3月首次发布以来，游戏开发者已经能够在创造精彩的体验之上，让玩家得以即刻沉浸在游戏中而不需要漫长的完整安装过程。玩家们也可以通过多种方式发现和访问免安装游戏，从Google Play中的"立即体验"按钮，到一条简单的网络链接，开发者们现在可以更轻松地吸引新玩家，并立即向他们展示自己的游戏。在这篇文章中，我们将向您展示如何使用Unity从头开始构建生产环境级别的免安装游戏，并会列举出免安装游戏为您带来的一些优势。

<font color="red">**采用免安装游戏的优势**</font>
![在这里插入图片描述](https://di.gameres.com/attachment/forum/201903/25/095257pgoe7g7u26lg3t6u.jpg)
**1.增加曝光**
免安装游戏可以让玩家更轻松地发现和体验您的游戏，只需单击一下"安装"按钮旁边的"立即体验"按钮，他们就可以从Google Play Store立刻启动您的游戏。玩家还可以点击网络广告等推广物料，从移动网站等位置直接进入您的游戏。
![在这里插入图片描述](https://di.gameres.com/attachment/forum/201903/25/095257tg8gg2m541q5s18w.jpg)
当然，好处远不止于此。用户还可以通过Google搜索、社交媒体、短信、电子邮件和许多其他平台分享的链接来进入您的游戏——只要能承载一条链接即可。
![在这里插入图片描述](https://di.gameres.com/attachment/forum/201903/25/095258zhs87aggwtnnttn7.jpg)
△能承载链接的介质很多，发挥想象力吧

![在这里插入图片描述](https://di.gameres.com/attachment/forum/201903/25/095258qtk7qld6w99qkwct.jpg)
**2.促进安装**

由于玩家在试玩之前没有"下载"的负担，这样一来他们就更容易用轻松的心态试玩您的游戏。另外，免安装也意味着玩家无需斟酌"这个游戏是否值得占用设备上的宝贵储存空间"。玩家可以试玩您游戏中最精华的部分，然后您可以提示让他们去安装完整版本——这个过程玩家甚至都不必离开当下的游戏体验，完全无缝。
![在这里插入图片描述](https://di.gameres.com/attachment/forum/201903/25/095259jwfv0colptfhzbvp.jpg)
**3.提高留存率**

免安装游戏可以为玩家提供机会立刻试玩您的游戏。玩家在试玩后才会主动决定安装完整游戏，这样一来，在下载后不久再卸载游戏的玩家数量就会减少。另外，由于有试玩这个过程做为起点，那些安装了完整游戏的玩家也更有可能乐在其中，从而帮助您增加随后下载游戏的玩家数量。

不少游戏开发者已经意识到了免安装游戏对他们现有游戏的影响:

- Hothead Games的游戏作品Mighty Battles的用户获取量增加了19%
- https://developer.android.google.cn/stories/instant-apps/mighty-battles
- King的游戏作品Bubble Witch 3 Saga的用户获取量增加
- https://developer.android.google.cn/stories/instant-apps/king
- Jam City的游戏作品Panda Pop得以找到高品质的玩家群体
- https://developer.android.google.cn/stories/instant-apps/panda-pop
- Playtika找到了新的玩家，提升了保留率，并增加了盈利
- https://developer.android.google.cn/stories/instant-apps/playtika


不少成功开发者都在通过免安装游戏迈向下一个成功，我们希望您也加入他们的行列:
![在这里插入图片描述](https://di.gameres.com/attachment/forum/201903/25/095259qbpeotvjaswtvjep.jpg)
<font color="red">**使用Unity打造免安装游戏难吗？**</font>
只需要几个小时，您就可以构建出免安装游戏并将其发布到内部测试轨道(internal test track)。如果使用Unity构建的游戏超过了13.5 MB限制，您可以在那里对您的免安装游戏进行内部测试和展示，您还会在Play Store中看到"立即体验"按钮。在内部测试轨道发布后，我们发现开发者们也会关注以下内容:

- 定制免安装游戏的外观。
- 将免安装游戏的体积减少到13.5 MB以下。(使用Unity构建免安装游戏的体积限制)
- 通过测试和QA运行免安装游戏，确保它完美适配沙盒需求和权限需求，并确保满足用户安全需求。(我们建议在不同版本的Android OS上进行测试，至少要包括Nougat和Oreo)


具体需要多长时间取决于游戏的实施细节和复杂程度。

**游戏沙盒需求**
https://developer.android.google.cn/topic/google-play-instant/getting-started/game-instant-app#target-sandbox-version

**游戏权限需求**
https://developer.android.google.cn/topic/google-play-instant/getting-started/instant-enabled-app-bundle#request-supported-permissions

<font color="red">**我具体应该怎么做？**</font>
![在这里插入图片描述](https://di.gameres.com/attachment/forum/201903/25/095259e72j88splpl27j3l.jpg)
创建自己的免安装游戏，需要以下五个步骤。

<font color="red">**第1步:设置您的工作环境**</font>

请先阅读这份检查清单，确保能顺利开始后续的操作:

- Unity 5.6或更高版本(我们建议至少采用2017.4发布版本)
- 适用于Unity的Google Play Instant插件


**Unity Asset Store**
https://assetstore.unity.com/packages/tools/integration/google-play-instant-plug-in-118292

**GitHub**
https://github.com/google/play-instant-unity-plugin

运行Android 5.0(Lollipop)或更高版本的Android设备(实体或模拟器均可)，启用了开发者模式和USB调试功能
游戏APK的项目源代码，用于编译测试和发布版本
在Google Play Console中创建内部测试轨道
注册加入Unity Development Beta(生产环境下必须加入)


**创建内部测试轨道**
https://support.google.com/googleplay/android-developer/answer/3131213#internal_test

**Unity Development Beta**
http://g.co/play/instantbeta
![在这里插入图片描述](https://di.gameres.com/attachment/forum/201903/25/095259rkvkij2quvk6xeau.jpg)
<font color="red">**第2步:构建和测试您的免安装游戏**</font>

接下来，您将把现有的游戏转换为免安装游戏。暂时不要考虑如何缩小体积或定制体验流程。

1.选择PlayInstant→Build Settings。
![在这里插入图片描述](https://di.gameres.com/attachment/forum/201903/25/095300q3dzrg3yjxrxrrik.jpg)
2.在构建设置弹出窗口中，将Android Build Type设置为Instant。

除非您已配置Digital Asset Link，否则请将Instant Apps URL字段留空。

如有必要，您可以在Override Scene字段中指定要使用的备用场景。

如果您使用的是Asset Bundles，请在AssetBundle Manifest字段中指定相应的manifest文件。

完成后单击"保存"。
![在这里插入图片描述](https://di.gameres.com/attachment/forum/201903/25/095300bzsx86lt50hvhc1q.jpg)
3.选择PlayInstant→Player Settings。在玩家设置弹出窗口中:

在运行免安装游戏之前，单击Required Changes列表中项目旁边的Update按钮以修复对应的设置条目。

我们还建议更新Recommended Changes中的设置，主要侧重于缩小体积。

完成后关闭弹出窗口。
![在这里插入图片描述](https://di.gameres.com/attachment/forum/201903/25/095300hbv8pa46b78ix4gy.jpg)
4.在免安装游戏配置完毕后，选择PlayInstant→Build and Run即可在已连接的设备上启动它。
![在这里插入图片描述](https://di.gameres.com/attachment/forum/201903/25/095301ntz4436h43jihvi6.jpg)
<font color="red">**第3步:上传到内部测试轨道**</font>

现在，将您在最后一步中编译出来的免安装游戏上传到内部测试轨道，这将允许最多100名选定的内部测试用户通过Play Store测试您的免安装游戏。

请记住，在内部测试轨道，您的免安装游戏无需满足13.5 MB的上传体积限制。

选择PlayInstant→Build for Play Console。

导航至Google Play Console，确保选择了相应的应用，然后导航至Android Instant Apps标签。

选择免安装应用内部测试(Instant app internal test)，然后按照该页面上的说明上传您刚创建的文件。

更多关于创建免安装应用的说明

https://support.google.com/googleplay/android-developer/answer/7381861?hl=en&ref_topic=7072031
![在这里插入图片描述](https://di.gameres.com/attachment/forum/201903/25/095301l0943i36ihi3jcq1.jpg)
注意:请确保您已正确配置内部应用测试人员列表，以便他们可以访问该游戏。您可以在App releases→Instant app internal test→Manage testers中设置人员。

![在这里插入图片描述](https://di.gameres.com/attachment/forum/201903/25/095301fymw6d9wsdst6suu.jpg)
<font color="red">**第4步:缩小文件大小以满足13.5 MB的体积限制**

现在您已经熟悉了构建免安装游戏的技术细节，现在您应该开始着眼于它的外观与体验细节，并着力缩小它的尺寸了。如果您的游戏文件体积很大，请不要担心——即便是游戏文件体积超过350 MB的开发者仍然能够推出免安装游戏。以下是一些有助于将文件体积减小到13.5 MB以下的技术:

优化玩家设置

正如我们之前看到的，PlayInstant→Player Settings提供了许多推荐的改动措施，以便减少游戏文件体积。请您进行以下更新:

图形API应限于OpenGLES2

Mono模式编译的项目应启用code stripping

IL2CPP模式编译的项目应启用engine stripping

简化游戏内容

不要把您的整个游戏都塞到免安装游戏里去。您的免安装游戏应该让玩家感受到完整游戏的一部分，例如一段教程，或是一两个令人兴奋的关卡。删除不需要的内容，例如材质、模型、图像或不属于游戏核心玩法的音频内容。

最后，您还可以通过优化下列资源来进一步缩小文件体积:

- 纹理的分辨率
- 3D模型中的多边形数量
- 音质


了解更多关于免安装游戏UX的最佳实践

https://developer.android.google.cn/topic/google-play-instant/best-practices/games

使用Asset Bundle

Asset Bundle允许您在免安装游戏运行时动态加载资源。如果资源直到运行时才被下载，就不会被计入13.5 MB的限制。这是游戏开发者减少免安装游戏体积的常用方法。以下三个要素不可或缺:

基于您的免安装游戏中的场景创建的Asset Bundle。

一个用户友好的加载屏幕，可以在下载Asset Bundle时告知用户。

用于放置和分发Asset Bundle的Web服务器或内容分发网络(CDN)——您自己的服务器，或者Google Cloud Storage以及任何第三方的CDN服务都可以。

接下来，我们将通过使用Google Play Instant Unity插件附带的Quick Deploy工具向您展示利用Asset Bundle功能的最快方法。

注意:虽然您可以使用任意数量的Asset Bundle，但每个Asset Bundle的体积必须小于15 MB。

1.选择PlayInstant→Quick Deploy。

2.选择"Bundle Creation"选项卡，然后选择要动态加载的场景。完成后，选择底部的Build AssetBundle，并将生成的文件上传到Web服务器或CD
![在这里插入图片描述](https://di.gameres.com/attachment/forum/201903/25/095301i6xth7ttl5t3h83w.jpg)
3.选择"Loading Screen"选项卡，然后设置Asset Bundle的URL，以及要用于加载屏幕的背景纹理(默认的那个就很好)。完成后，选择Create Loading Scene。

![在这里插入图片描述](https://di.gameres.com/attachment/forum/201903/25/095302gwhhaqrqzzwe8afe.jpg)
现在您已经得到了一个加载屏幕，它将用于动态加载您的Asset Bundle。这个加载屏幕的源代码已经由Unity自动生成，您可以进入到这个新的场景中查看细节并进行定制。
![在这里插入图片描述](https://di.gameres.com/attachment/forum/201903/25/095302devwtq8giiesldg0.jpg)
<font color="red">**第5步:将体验用户转化为安装用户**

最后，您需要为玩家建立一种从免安装游戏中获得完整游戏的方式。如有必要，您可以使用Google Play Instant Plugin for Unity附带的Cookie API无缝迁移玩家数据。

<font color="red">**从免安装游戏升级到完整游戏**

您的免安装游戏应该为玩家提供安装完整游戏的入口，比如不影响游戏体验的消息或按钮入口。在玩家确定要安装完整游戏时，调用由Play Instant Plugin提供的ShowInstallPrompt方法，将安装过程移交给Google Play:

**将体验玩家的数据迁移至完整游戏(可选)**

在某些情况下，您可能需要迁移玩家在体验时产生的一些信息。例如:

- 当玩家购买了游戏中的物品，升级或定制了他们的角色时
- 如果免安装游戏与完整游戏中的教程环节类似，玩家可以在安装完整版本后选择跳过教程环节
- 在玩家从免安装游戏转换为完整游戏后为他们提供奖励


您可以使用随Google Play Instant插件一起提供的Cookie API，这样您就可以在安装之前和之后轻松地写入和读取数据:

1.调用CookieApi.SetInstantAppCookie以便在免安装游戏中存储数据:
![在这里插入图片描述](https://di.gameres.com/attachment/forum/201903/25/095945o395djuyjebgy9tj.jpg)
2.在完整游戏中调用CookieApi.GetInstantAppCookie以读取数据:

![在这里插入图片描述](https://di.gameres.com/attachment/forum/201903/25/095945dk1rykbu7tg8kesq.jpg)
就是这些！相信您已经对如何创建免安装游戏，以及使用免安装游戏所带来的好处有了比较全面的认识。我们期待着更多用户通过免安装游戏来快速体验您的作品，更期待着他们会因为免安装游戏成为完整游戏的忠实玩家。祝大家游戏开发路上好运！
