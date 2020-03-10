---
layout: post
category: web
title: 【框架】ROBOTLEGS轻量级AS3框架
tagline: by 恬静的小魔龙
tag: Other
---

>任何一个好的东西（语言、框架等）最终还取决于用的人
语言和框架本身并不能保证用户的代码清晰、解耦等，
当然它只是尽可能地做到这点。


## 前言
Robotlegs是一个用来开发Flash，Flex和AIR应用的纯AS3微架构(框架)。Robotlegs专注于将应用程序各层排布在一起并提供它们相互通讯的机制。Robotlegs试图通过提供一种解决常见开发问题的经过时间检验的架构解决方案来加速开发。Robotlegs无意锁定你到框架，你的类就是你的类的样子，而且应该很容易地切换到其他框架。

框架提供一个基于Model-View-Controller元设计模式的默认实现。这个实现提供一个针对应用程序结构和设计的强烈建议。虽然它确实轻微减低了你的应用程序的便携性，不过它依然以最低限度影响你的具体类为目标。通过扩展MVCS实现类，你可以获得很多有用的方法和属性。

你不必使用Robotlegs的标准MVCS实现。你可以使用它的任意部分，或者完全不使用它，或者使用自己的实现来适应你的需求。它是为了提供合适的参考实现和快速开始使用Robotlegs而被包含进来。


## 1.MVC
![在这里插入图片描述](https://images.cnblogs.com/cnblogs_com/skynet/201203/201203211555447330.png)
MVC就是(Model View Controller)模型－视图－控制器，是一个设计模式（也可以称为架构模式），但目前在大部分人眼里显然并不只有这点意义。受到各种MVC框架的影响，消息通信，依赖注入，隔离编译等等概念现在都加入到了MVC的概念里，而且因为应用上的重叠和相似性，还常常和三层架构的概念混淆。


### 1.1.     MVC分解
对于一个用于显示的程序，最开始，只有View（视图）。

我们要获得一个数据并显示，就是写段代码调用接口，获得数据，然后根据数据更新视图。而这里的数据只是一个临时变量，或者存在于视图的某个属性中。显然，这样做的话，数据是依赖于视图的，如果不通过视图，或者视图根本不存在的时候，其他模块就访问不到这个已经返回的数据，则需要重新调用一次，这显然是一种浪费，而且也不便利。因此我们在这里将数据保存到别处，和视图不共享同一生命周期，那么这分离出来的数据则被规定为Model（模型）。

（在这里我必须打断说明下，模型!=数据，仅仅是在这个简化例子里模型==数据。模型是为视图提供内容的单独模块，具体内部是什么情况则是各种各样的，提供数据只是它的功能之一）

至此，就是最基本的分离表现层的原则。然而，有个地方却是分不开的——原来控制加载数据的这部分代码。它负责的是获取数据，并指示视图根据数据更新的任务，而它的去向有两个选择：要不加到视图上（这是通常的做法），要不加在模型上（如果需要模型对应多个视图），反正他不可能因为分离就凭空消失。那么我们把这部分代码提出来作为单独的部分，就是Controller（控制器）。

一旦分离出了控制器，联系模型和视图的具体代码就不再处于两者之上（当然调用代码还是有的），而是一个独立的部分。它有自己的名字，而且不会和其他不相关的内容混合在一起，非常清晰，容易定位。而模型和视图从此也只关心控制器，而不关心对方。他们的代码都是处理自己的事情，别人的事情全部交给控制器去办，这使得他们自己的功能也非常独立，减少了需要考虑的要素。

这只是代码位置的移动，但是这样移动后，将牵涉内容最多最易变，但是代码量最少的代码单独放入控制器中，并妥善管理。相当与将分散在房间各处的开关集中于一处，排列整齐并标注名字，做成遥控器在手里把玩，从此就能从奔波于房间各个位置，寻找隐藏在角落里的小突起的日常活动中解放出来。其意义，不言而喻。

而这，作为MVC的作用，已经足够了。


### 1.2.     消息通信、依赖注入等
一旦我们将视图，模型，控制器分离后，可以做到什么呢？

因为视图和模型都只用处理自己的事情，对控制器的调用只是一句代码而已，那么，它们实际上就可以被隔离出去。负责这部分代码的人只需要关心自己的事情，而不需要关心整个环境。同样的，编写控制器的人只需要知道视图和模型的对外接口，而不需要了解它们的具体实现，也就是只需要关心环境。这使得不同开发者需要的知识被分隔开，每人只需要了解有限的少量知识，最终却又能顺利合并在一起。

这是就是协作分工的基础。

在这之后，三者的关系只存在简单的调用代码。那么为了能够彻底的分离和解耦，就可以将调用代码改为发送消息或者其他的动态形式，这样就能在没有其他部分的时候独立编译。由不同人在不同的工作环境独立完成自己的部分，并在最后发布时候简单合并在一起，这确实是最理想的协作模式。

做到这点需要一个消息框架，而这就是MVC框架的主要任务。除此之外，各种不同的框架还会加入其它设计模式，提供各种附加功能，自动依赖注入就是其中一项。还可能加入其它的中间件，进一步分割层次，诸如分离出视图其中的逻辑部分，使得绘图和位置代码不会和逻辑代码混合，方便分工以及修改。使用观察者模式，使得数据部分的修改可以自动同步到视图上，如此这般……MVC框架指的是“实现MVC的框架”，而非“只实现MVC的框架”，仅仅实现MVC，这个框架的功能太过贫乏了，毕竟MVC也就是那种程度的东西。

最终，完成了这些之后，就成为了一般人表面看到的东西。虽然各式各样，我们都将其称之为“MVC框架”。


## 2.  RobotLegs与其它框架的比较

下面对比下比较热门的几个as3框架：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190403153627341.png)
“pureMVC和Cairngorm是两个较早出现的框架，目前我不建议再使用它们。pureMVC的问题在于过于强调分离而缺乏实际功能，提供的便利很难抵消它本身的消耗，性价比较低。Cairngorm的问题则在于过于强调模型更新视图的流程，限制太多，灵活程度不够。

后出的几个框架就好多了，Mate使用了一个全局事件定义，配合FLEX写法非常简略。Swiz则是用控制反转+依赖注入，也就是Spring的做法，而且元标签注入的方式很有趣，感兴趣的可自行查阅资料。”

RobotLegs是一个和Swiz非常相似的框架，但也有一些自己的特点。RobotLegs它是基于pureMVC的，可以像pureMVC这样来使用它，对于使用pureMVC的团队它是很容易接受的代替品。pureMVC是基于Notification的一个MVC框架，主要目的是为了各个部分能够解耦，当然它也基本上能够做到。RobotLegs则是基于消息以及消息携带的数据等来实现解耦。RobotLegs是基于pureMVC的思想，但是在一些方面更加出色，例如消息的强类型，依赖注入方式，消息携带数据等等。

“robotlegs里使用了flash的事件机制来通信，而puremvc使用自定的通知来发消息。这里区别不大，只是使用事件机制就得写事件类;然后robotlegs使用自动mediator自动注册，它靠侦听addtostage来处理，当然，手动注册也是允许的。这样方便了不少，puremvc只能手动在视图组件初始化时注册，而且有时有些内部组件经常会出现未初始化完成时就去注册，导致访问不到this。还有最重要的依赖注入，robotleg不再使用puremvc那样的传递参数方法，而是使用依赖注入，包括mediator对view组件的引用都是注入的。这样依赖性又小了很多，感觉非常不错。”

个人认为Robotlegs比pureMVC好用，Robotlegs是pureMVC的改进升级版而且是专注于as3的，注入技术更是省去了pureMVC的许多麻烦。

## 3.  RobotLegs的依赖注入
RobotLegs使用了以下3个面向对象设计模式：

1.  自动依赖注入（Automated Dependency Injection），不用对象自己创建、获取依赖的对象，而是由容器/框架来完成。

2.  命令模式（Command Pattern），

	2.1  命令模式的本质是对命令进行封装，将发出命令的责任和执行命令的责任分割开；

	2.2  每一个命令都是一个操作：请求的一方发出请求，要求执行一个操作；接收的一方收到请求，并执行操作。

	2.3命令模式允许请求的一方和接收的一方独立开来，使得请求的一方不必知道接收请求的一方的接口，更不必知道请求是怎么被接收，以及操作是否被执行、何时被执行，以及是怎么被执行的。

3.  调度者模式（Mediator Pattern），定义一个中介对象来封装系列对象之间的交互。中介者使各个对象不需要显示地相互引用，从而使其耦合性松散，而且可以独立地改变他们之间的交互。

Robotlegs围绕依赖注入设计模式展开。最简单地，依赖注入是为对象提供实例变量或属性的行为。

当你传递一个变量到一个类的构造函数，你在使用依赖注入；

当你设置一个类的属性，你在使用依赖注入；

如果你不是使用严格的过程或线性方式编写AS3，很可能你现在就在使用依赖注入。

Robotlegs使用基于元数据的自动依赖注入。这是为了方便开发而提供，而且在排布应用程序并提供类和它所需要的依赖时，可以减少很多代码量。虽然完全可以手动提供这些依赖，但是允许框架来履行这些职责可以减少出错的机会，并且通常可以加快编码进程。

### 3.1.        依赖注入
依赖注入（Dependency Injection，简称DI），是一个重要的面向对象编程的法则来削减计算机程序的耦合问题。依赖注入还有一个名字叫做控制反转（Inversion of Control，英文缩写为IoC）。依赖注入是这样一个过程：由于某客户类只依赖于服务类的一个接口，而不依赖于具体服务类，所以客户类只定义一个注入点。在程序运行过程中，客户类不直接实例化具体服务类实例，而是客户类的运行上下文环境或专门组件负责实例化服务类，然后将其注入到客户类中，保证客户类的正常运行。即对象在被创建的时候，由一个运行上下文环境或专门组件将其所依赖的服务类对象的引用传递给它。也可以说，依赖被注入到对象中。所以，控制反转是，关于一个对象如何获取他所依赖的对象的引用，这个责任的反转。

![在这里插入图片描述](https://images.cnblogs.com/cnblogs_com/skynet/201203/20120321155544296.png)
依赖注入使用以下三种基本技术：

     	类型1 (基于接口)： 可服务的对象需要实现一个专门的接口，该接口提供了一个对象，可以从用这个对象查找依赖(其它服务)。

	  类型2 (基于setter)： 通过属性的setter方法为可服务对象指定服务。

	  类型3 (基于构造函数)： 通过构造函数的参数为可服务对象指定服务。


>不要重复发明轮子！
对于应用频繁的需求，总是有人设计各种通用框架和类库以减轻人们的开发负担。例如，数据持久化是非常频繁的需求，于是各种ORM框架应运而生；再如，对MVC的需求催生了Struts等一批用来实现MVC的框架。


随着依赖注入变成了非常频繁的需求，而如果全部手工完成，不但负担太重，而且还容易出错。再加上反射机制的发明，于是，自然有人开始设计开发各种用于依赖注入的专用框架。这些专门用于实现依赖注入功能的组件或框架，就是IoC Container。

IOC关注服务(或应用程序部件)是如何定义的以及他们应该如何定位他们依赖的其它服务。所以通常，通过一个容器或定位框架（著名的Spring容器、）来获得定义和定位的分离，容器或定位框架负责：

l  保存可用服务的集合

l  提供一种方式将各种部件与它们依赖的服务绑定在一起

l  为应用程序代码提供一种方式来请求已配置的对象(例如，一个所有依赖都满足的对象)， 这种方式可以确保该对象需要的所有相关的服务都可用。

### 3.2.        SwiftSuspenders
Robotlegs采用一种适配器(adapter)机制来为框架提供依赖注入机制。默认地，框架配备了SwiftSuspenders注入/反射库来适合这个要求。另有SmartyPants-IoC 和 Spring Actionscript 的适配器可以使用。可能有潜在的特定需求来使用其它的依赖注入适配器，但是如果没有特别的理由，建议使用默认的SwiftSuspenders，因为它为 Robotlegs 做了一些特别调整。
![在这里插入图片描述](https://images.cnblogs.com/cnblogs_com/skynet/201203/201203211555457689.png)
SwiftSuspenders 适配器注入语法
SwiftSuspenders 支持三种类型的依赖注入：

	  属性(域)注入

  	参数(方法/设值) 注入

	  构造注入

我们将特别介绍属性注入，以及在 Robotlegs 里如何使用。将属性注入类有两种选择，你可以使用未命名，或命名的注入：

**[Inject]
public var myDependency：Depedency; //未命名注入
[Inject(name="myNamedDependency")]
public var myNamedDependency：NamedDepedency; //命名注入**

Robotlegs里三处提供了注入映射：MediatorMap、CommandMap、直接通过Injector。MediatorMap、CommandMap也都是使用Injector，但它们同时做了一些各自层(tier)所需要的额外工作。顾名思义，MediatorMap用来映射Mediator；CommandMap用来映射Command；其它所有需要被注入的内容 (包括但不限于 Model) 都要直接使用 Injector 映射。


<font color="blue">Injector 类的映射注入</font>
具体 Injector 类的适配器都遵照 IInjector 接口。 这个接口为不同的依赖注入解决方案提供了统一的API。 本文专注于 SwiftSuspenders，但这些语法同样适应于其它任何遵照 Iinjector 接口的 Injector。

Injector是你应用程序里所发生的所有依赖注入的生产车间。它用来注入框架 actor，同时也可以用来执行你的应用程序所需要的任何其它注入。这包括但不限于 RemoteObjects，HTTPServices，工厂类，或者事实上任何有可能成为你的对象所需要的依赖的类/接口。

下面是实现 IInjector 接口的类所提供的四个映射方法：
![在这里插入图片描述](https://images.cnblogs.com/cnblogs_com/skynet/201203/201203211555492865.png)
<font color="blue">MediatorMap 类的依赖注入</font>
MediatorMap 实现 IMediatorMap 接口。 IMediatorMap 提供两个方法来将你的 mediators 映射到 view 并注册它们以便用来注入。

mapView(viewClassOrName：*，mediatorClass：Class，injectViewAs：Class = null，autoCreate：Boolean = true，autoRemove：Boolean = true)：void

mapView 接受一个视图类，MyAwesomeWidget，或者一个视图的类全名，com.me.app.view.components::MyAwesomeWidget 作为第一个参数。 第二个参数是将要作为视图组件中介的 Mediator 类。 [injectViewAs 内容未完成]，最后的两个参数 autoCreate 和 autoRemove 提供方便的自动管理 mediator 的布尔值开关。

//在你的程序里某个映射/配置发生的地方

mediatorMap.mapView(MyAwesomeWidget，MyAwesomeWidgetMediator);

//在conntextView 的显示列表里的某个地方

var myAwesomeWidget:MyAwesomeWidget = new MyAwesomeWidget();

this.addChild(myAwesomeWidget); // ADDED_TO_STAGE 事件被抛出，触发这个视图组件的中介机制

这个方法使用了自动中介机制。

<font color="blue">CommandMap 类的依赖注入</font>
CommandMap 类实现 ICommandMap 接口，提供一个用来将 command 映射到到触发它们的框架事件的方法。

mapEvent(eventType：String，commandClass：Class，eventClass：Class = null，oneshot：Boolean = false)

你要提供给 commandMap 一个可以执行的类，执行它的事件类型，可选的这个事件的强类型，以及这个 command 是否只被执行一次并且随即取消映射的布尔值开关。

这个可选的强类型事件类用来对Flash平台的"magic string"事件类型系统做额外的保护。以避免采用相同事件类型字符串的不同事件类之间的冲突。

//在你的程序里某个映射/配置发生的地方

commandMap.mapEvent(MyAppDataEvent。DATA_WAS_RECEIVED，MyCoolCommand，MyAppDataEvent);

//在事件被广播的另外一个框架actor里

//这触发了随后被执行的被映射的 command

dispatch(new MyAppDataEvent(MyAppDataEvent.DATA_WAS_RECEIVED，someTypedPayload))
## 4.  Robotlegs的标准MVCS实现
Robotlegs提供了一个比较规范的MVC+S（Model模型，View视图，Controller控件和Service服务）实现，来帮助开展工作。通过将几个经过时间检验的设计模式整合到一个具体实现， Robotlegs 的 MVCS 实现可以用做创建你的应用程序的一致方案。 通过这些架构概念着手一个应用程序， 你甚至可以在开始你的设计之前避免很多常见的障碍：

  分离：MVCS 提供一种将你的应用程序分离到提供特定功能的无关联的层的很自然的方法。 view 层处理用户交互。 model 层处理用户创建的或从外部获取的数据。 controller 提供一种封装各层之间复杂交互的机制。 最后， service 层提供一种和外界(比如远程服务 API 或文件系统)交互的独立机制。

  组织：通过这种分离我们自然获得一个组织水平。 每个项目都需要某个组织水平。 是的，有人可以把他们所有的类都扔到顶级包下完事，但即使是最小的项目这也是不可接受的。 当一个项目有了一定的规模就需要开始组织类文件的结构了。 当向同一个应用程序开发中增加团队成员的时候问题就更加严重了。 RobotLegs 的 MVCS 实现为项目描绘出一个分为四层的优雅的组织结构。

  解耦：Robotlegs 的MVCS实现将应用程序解耦为4层。 每层都与其它层隔离， 使分离类和组件分别测试变得非常容易。除了简化测试进程， 通常也使类更具便携性以在其它项目中使用。 比如， 一个连接到远程 API 的 Service 类可能在多个项目中都很有用。 通过解耦这个类， 它可以不需重构便从一个项目转移到另一个中使用。

### 4.1.        Robotlegs MVC+S
一个典型的Robotlegs MVC+S应用程序包含以下几个部分：
![在这里插入图片描述](https://images.cnblogs.com/cnblogs_com/skynet/201203/201203211555523964.png)
  Context：所谓Context（上下文），实际上是一套自展机制，用来初始化Robotlegs所使用的依赖注入以及各种核心工具。

  Commands：所谓Commands（命令），代表的是应用程序所能执行的独立操作。通常，Commands（命令）会作为对用户操作的反应，但命令的作用并不仅限于此。

  Mediators：所谓Mediators（中介），是用来管理应用程序中的视图组件与应用程序中的其它对象之间的信息交流。

  Model：Models（模型）中保存着数据信息，并且表现出应用程序当前的状态。

  Service：Services（服务），是应用程序与外界的接口。

下面用用互操作view到得到响应的一个流程图：

![在这里插入图片描述](https://images.cnblogs.com/cnblogs_com/skynet/201203/201203211555573402.png)
### 4.2.        推荐目录结构
Robotlegs代码包结构组织：

```csharp
[domain.lib]

    various utilities

[domain.project]

    [projectmodule]

        [model]

            [events]

            [vo]

            ProjectModuleStateModel

        [view]

            [events]

            [renderers]

            [skins]

            MyProjectModuleView

            MyProjectModuleViewMediator

        [controller]

            [startup]

            MyProjectModuleActionCommand

        [service]

            [helpers]

            MyProjectModuleService

            IProjectModuleService

        [signals]

    [projectmodule]

        [model]

            [events]

            [vo]

            ProjectModuleStateModel

        [view]

            [events]

            [renderers]

            [skins]

            MyProjectModuleView

            MyProjectModuleViewMediator

        [controller]

            [startup]

            MyProjectModuleActionCommand

        [service]

            [helpers]

            MyProjectModuleService

            IProjectModuleService

        [signals]

    …
```
这是Joel Hooks建议的包结构。这是一个多模块情况的包结构，如果是单模块，可以少去projectmodule这一层。[signals]包不是必须的，除非你用到了Signals。

把每个模块要都用到的公共代码放到[domain.lib]下。模块和模块内代码保证独立，不会相互使用（引用，实例化）。

StartupCommand内代码建议以MVC结构分离。如果一个模块中StartupCommnad内有过多代码，建议把它拆分成ModelStartupCommand，ControllerStartupCommand和ViewStartupCommand，这样职责更清晰。

[controller]和[view]中，如果有很多类，建议按照功能建立子目录。

### 4.3.        Context
实际上是一套自展机制，用来初始化Robotlegs所使用的依赖注入以及各种核心工具，一个应用程序是可以有多个 context 的，这对想要加载外部模块的应用程序很有用，因为在一个 context 里的 actor 只能在他们的 context 定义的范围之内相互通讯，所以在一个模块化的应用程序里，不同context 之间的通讯是完全可能的。

每一个Robotlegs项目都以Context开始，只有Context实例化之后，Robotlegs才能启动和运行。创建Context需要给提供一个根视图并调用startup()方法。

<font color="blue">提供根视图（root-view）</font>
每个Rogotlegs应用程序需要一个根视图——DisplayObjectContainter的一个实例。它将供mediatorMap使用，所以当子视view图添加到根视图时，对应的mediator会被创建并和view关联起来。

<font color="blue">调用startup()方法</font>
启动应用程序需要在所有配置都准备好之后，然后调用startup()方法来启动。可以通过将autoStartup:Boolean=true，然后该方法会自动被调用；又或者将autoStartup:Boolean=false，然后手动调用startup()方法。

在startup()方法中会初始化依赖注入规则！如果规则很少，可以全部写在这个该方法中；否则，建议分离到不同的配置类中。

### 4.4.        Command
Controller 层由 Command 类体现（一个Command是一个简明、单一目的的控制器controller对象）。Command是短生命周期的无状态对象。它们在被实例化和执行之后立即释放。Command 应该只在处理框架事件时被执行，而不应该被任何其他框架 actor 实例化或执行。

![在这里插入图片描述](https://images.cnblogs.com/cnblogs_com/skynet/201203/201203211556002516.png)

<font color="blue">Command 职责

Command 用于应用程序各层之间相互通讯，也可能用来发送系统事件。这些系统事件既可能发动其它的 Command， 也可能被一个 Mediator 接收，然后对一个 View Component 进行对应这个事件的工作。

Command 被 Context 的 CommandMap 注册到 Context。CommandMap 在 Context 和 Command 类里默认可用。Command 类被注册到 Context 时接收4个参数: 一个事件类型; 响应这个事件时执行的 Command 类; 可选的事件类; 一个是否该 Command 只被执行一次随即被取消注册而不响应后续事件触发的一次性设置.

<font color="blue">触发 Command

Command 被 Mediators，Services，Models，和其它 Command 广播的框架事件触发。一个被映射的 command 在响应一个框架事件时被实例化，所有已被映射，并被 [Inject] 元数据标签标记过的依赖都会被注入到这个 Command。另外，触发这个 Command 的事件实例也会被注入。当这些依赖被注入完毕，Command 的执行方法会被自动调用，Command 便会进行它的工作。你不需要，而且不应该直接调用 execute() 方法。这是框架的工作。

<font color="blue">应用程序层的解耦

Command 是解耦一个应用程序里各个 actor 的非常有用的机制。因为一个 Command 永远不会被 Mediator，Model 或者 Service 实例化或执行，这些类也就不会被耦合到 command，甚至都不知道 command 的存在.

为了履行它们的职责，Command 可能:

  映射 Mediator，Model，Service，或者 Context 里的其它 Command

  广播可能被 Mediator 接收或者触发其它 Command 的事件.

  被注入Model，Service，和Mediator 以直接进行工作.

需要注意的是，不建议在一个 Command 里直接和 Mediator 交互。虽然这是可行的，但会将这个 Mediator 耦合到这个 Command。因为 Mediator 不像 Service 和 Model，它可以接受系统事件，更好的做法是让 Command 广播事件，然后让需要响应这些事件的 Mediator 监听它们。

### 4.5.        View & Mediator
View 由 Mediator 类体现。继承 Mediator 的类管理应用程序中的 View Component 与应用程序中的其它对象之间的信息交流。一个Mediator 将会监听框架事件和 View Component 事件，并在处理所负责的 View Component 发出的事件时发送框架事件。这样开发者可以将应用程序特有的逻辑放到 Mediator，而避免把 View Component 耦合到特定的应用程序。

注意，Mediator仅仅是框架和View Component之间的桥梁，这也是他唯一的职责。

![在这里插入图片描述](https://images.cnblogs.com/cnblogs_com/skynet/201203/201203211556024335.png)
<font color="blue">Mediator 职责 

一个 View Component 是任何的UI组件和/或它的子组件。一个 View Component 是已被封装的，尽可能多地处理自己的状态和操作。一个 View Component 提供一个包含了事件，简单方法和属性的API。Mediators负责代表它所中介的View Component和框架交互。这包括监听组件及其子组件的事件，调用其方法，和读取/设置组件的属性.

一个 Mediator 监听它的 View Component 的事件，通过 View Component 暴露的 API 访问其数据。一个 Mediators 通过响应其它框架 actor 的事件并对自己的 View Component 进行相应修改来代表它们。一个 Mediator 通过转发 View Component 的事件或自己向框架广播合适的事件来通知其它的框架 actor.

<font color="blue">映射一个 Mediator

任何可以访问到mediatorMap实例的类都可以映射 Mediator。这包括 Mediator，Context，和 Command 类.

这是映射一个 mediator 的语法:

mediatorMap.mapView( ViewClass，MediatorClass，autoCreate，autoRemove );

当view添加到舞台上时，Mediator会被自动创建并关联起来。

![在这里插入图片描述](https://images.cnblogs.com/cnblogs_com/skynet/201203/20120321155605102.png)
当view从舞台上移除时，Mediator会被自动清除。
![在这里插入图片描述](https://images.cnblogs.com/cnblogs_com/skynet/201203/201203211556086183.png)
<font color="blue">访问一个 Mediator 的 View Component 

当一个 View Component 在一个 Context 的 contextView 里被添加到舞台上的时候，它默认地会被根据 MediatorMap 做映射时的配置被自动关联。在一个基本的 mediator 里，viewComponent会被注入为被中介的 view component。一个 Mediator 的viewComponent属性是 Object 类型的。在大多数情况下，我们希望访问一个强类型的对象以从中获益。为此目的，我们注入被中介的 view component 的强类型实例:

```csharp
public class GalleryLabelMediator extends Mediator implements IMediator

{

      [Inject]

      public var myCustomComponent:MyCustomComponent;

            

      /**

      * 覆写 onRegister 是添加此 Mediator 关心的任何系统或 View Component 事件的好机会.

      */

      override public function onRegister():void

      {

             //添加一个事件监听器到 Context 来监听框架事件

             eventMap.mapListener( eventDispatcher，MyCustomEvent.DO_STUFF，handleDoStuff );

             //添加一个事件监听器到被中介的 view component

             eventMap.mapListener( myCustomComponent，MyCustomEvent.DID_SOME_STUFF，handleDidSomeStuff)

      }

     

      protected function handleDoStuff(event:MyCustomEvent):void

      {

             //把事件的强类型负载设置到 view component 的属性。

             //View component 很可能基于这个新数据管理自己的状态.

             myCustomComponent.aProperty = event.payload

      }

     

      protected function handleDidSomeStuff(event:MyCustomEvent):void

      {

             //把这个事件转发到框架

             dispatch(event)

      }

}
```
通过这种方法我们现在可以很方便地访问被中介的 view component 的公开属性和方法.

<font color="blue">给一个 Mediator 添加事件监听

事件监听器是 Mediator 的眼睛和鼻子。因为框架内的所有通讯都通过原生的Flash事件，Mediator 可以通过添加事件监听器来响应感兴趣的事件。除了框架事件，Mediator同时监听所中介的 view component 的事件.

通常在 Mediator 的 onRegister 方法里添加事件监听。在 Mediator 生命周期中的这个阶段，它已经被注册并且它的 view component 和其它依赖也都已被注入。具体的 Mediator 类必须覆写 onRegister 方法。也可以在其它方法里添加事件监听，比如响应框架事件和 view component 事件的事件处理方法里.

Mediators 装备了一个有 mapListener() 方法的 EventMap。这个方法注册每个被添加到 Mediator 的事件监听，并且确保 mediator 被框架取消注册时删除这些事件监听。Flash 里删除事件监听是很重要的，因为如果一个类里添加了事件监听而没有删除，Player将无法对此类进行运行时垃圾回收(GC，Garbage Collection)。也可以使用传统的 Flash 语法添加事件监听器，但要注意也要手动把它们删除。

![在这里插入图片描述](https://images.cnblogs.com/cnblogs_com/skynet/201203/201203211556117489.png)
<font color="blue">监听框架事件

所有框架里的actor在实例化时都会被注入一个eventDispatcher属性。这个eventDispatcher就是 Mediator 发送和接受框架事件的机制.

eventMap.mapListener(eventDispatcher，SomeEvent.IT_IS_IMPORTANT，handleFrameworkEvent)

通过此语法，一个 Mediator 现在监听了SomeEvent.IT_IS_IMPORTANT事件并在handleFrameworkEvent方法里处理它。

<font color="blue">广播框架事件

Mediator的一个很重要的职责就是向框架发送其它 actor 可能感兴趣的事件。这些事件通常是在响应应用程序用户和被中介的 view component 之间的一些交互时发出的。这里同样有一个可以减少发送事件到框架的代码输入的很有用的方法:

dispatch(new SomeEvent(SomeEvent.YOU_WILL_WANT_THIS，myViewComponent.someData))

这个事件现在可以被其它 Mediator 接收或者执行一个 command 了。发出事件的 Mediator 并不关心其它的 actor 如何回应这个事件，它只是简单地广播一条有事发生的消息。一个 mediator 也可以监听自己发出的事件，然后据此作出回应.

<font color="blue">监听 View Component 事件

Mediator 负责所中介的 view component 发出的事件。这可以是个独立组件，比如 TextField 或者 Button，也可以是有嵌套层级的复杂组件。当 mediator 收到 view component 发出的事件会使用指定的方法处理它。和框架事件一样，EventMap 的 mapListener 方法是给一个 mediator 添加事件监听的首选.

eventMap.mapListener(myMediatedViewComponent，SomeEvent.USER_DID_SOMETHING，handleUserDidSomethingEvent)

响应一个 view component 的事件时，一个 mediator 可能:

  考察事件的负载payload (如果有)数据

  考察 view component 的当前状态

  对view component 进行需要的工作

  发送系统事件以通知其它actor有事发生

通过 Mediator 访问 Model 和 Service
你的 mediator 可以监听 Service 和 Model 类派出的系统事件来提高松耦合性。通过监听事件，你的 mediator 不需要关心事件来源，而只需直接使用事件携带的强类型的负载payload数据。因此，多个 mediator 可以监听相同的事件然后根据所收到的数据调整自己的状态.

在一个 mediator 里直接访问 service 可以提供很大便利而不会带来严重的耦合性问题。一个 service 并不存储数据，只是简单地提供一个向外部service发送请求并接受响应的API。能够直接访问这个API可以避免在你的应用程序中增加不需要的 command 类来达到同样目的。如果这个 service API 在很多 mediator 中通过相同的方式访问，将此行为封装到一个 command 里有益于保持此行为的一致性并减少对此 service 的反复调用以及在你的 mediator 里的直接访问.

建议通过 model 和 service 实现的接口将 model 和 service 注入 mediator。

>访问其它 Mediator
如同 Service 和 Model,在一个 Mediator 里也可以注入和访问其它的 Mediator。这种做法是 强烈不建议的 因为这种紧耦合可以简单地通过使用框架事件进行通讯而避免。
.

### 4.6.        Models
Model 类用来管理对应用程序的数据模型的访问。Model 为其它框架actor提供一个 API 来访问，操作和更新应用程序数据。这个数据包括但不限于原生数据类型比如 String，Array，或者像 ArrayCollection 一样的域特有对象或集合.

Model 有时被当做简单的 Model 比如 UserModel，有时也被当做 Proxy 比如 UserProxy。在 Robotlegs 里，这两种命名都是用作相同的目的，为应用程序数据提供一个 API。不管采用哪种命名 model 都继承提供了核心框架依赖和一些有用方法的 Actor 基类。

Model 会在对数据模型进行某些工作之后发出事件通知。Model 通常具有极高的便携性。
<font color="blue">Model 职责

Model类封装了应用程序数据模型并为其提供一个 API。一个 Model 类是你的应用程序数据的看门人。应用程序里的其它 actor 通过 Model 提供的 API 请求数据。因为数据是通过 Model 更新，Model 装备了向框架广播事件的机制以向其它 actor 通知数据模型的变化使它们得以据此调整自己的状态.

除了控制对数据模型的访问，Model 通常也被用来保证数据状态的有效性。这包括对数据进行计算，或域特有逻辑的其它领域。Model 的这个职责非常重要。Model 是应用程序中最有潜力具有便携性的层。通过把域逻辑放入 Model，以后的 model 实现就不再需要像把域逻辑放入 View 或 Controller 层那样重复这些相同的逻辑，作为一个例子，你的 Model 里可能执行购物车数据的计算。一个 Command 将会访问这个方法，最终的计算结果将会被作为被某个 Mediator 监听的事件派发出去。这个 mediator 将会根据这个被更新的数据更新自己的 view component，应用程序的第一个迭代是个典型的 Flex 程序。这个计算也很容易在一个 Mediator 甚至视图里进行。应用程序的第二个迭代是一个需要全新视图元素的移动设备 Flash 应用程序。因为这个逻辑在 Model 里，所以可以很容易被两个完全不同元素的视图复用.

<font color="blue">映射一个 Model

Injector 有几个方法可以用来将你的 Model 类映射到你的框架actor。另外，这些方法事实上可以用来注入任何类到你的类里.

将一个已存在的实例当做一个单例注入映射，使用下面的语法:

injector.mapValue(MyModelClass，myModelClassInstance)

为每个注入映射一个类的新实例，使用下面的语法：

injector.mapClass(MyModelClass，MyModelClass)

另外，这也可以用来使用被注入的实现某接口的合适的类来映射这个用来注入的接口.

injector.mapClass(IMyModelClass，MyModelClass)

为某个接口或类映射一个单例实例，使用下面的语法:

injector.mapSingleton(MyModelClass，MyModelClass)

需要注意重要的一点，当提及上面的一个单例时，它并不是一个单例模式的单例。在这个 Context 之外并不强制它作为一个单例。Injector 简单地确保这个类的唯一一个实例被注入。这对处理你的应用程序数据模型的 Model 非常重要.

<font color="blue">从一个Model里广播事件

Model 类提供一个方便的dispatch方法用来发送框架事件:

dispatch( new ImportantDataEvent(ImportantDataEvent.IMPORTANT_DATA_UPDATED))

有很多理由派发一个事件，包括但不限于:

  数据已被初始化并准备好被其它 actor 使用

  一些数据片被添加到 Model

  数据被从 Model 中删除

  数据已改变或者更新

  数据相关的状态已改变

>在一个 Model 里监听框架事件
虽然技术上可能，但强烈不建议这样做。不要这样做。只是为了说清楚：不要这样做。如果你这样做了，不要说你没被警告过.

### 4.7.        Services
Service 用来访问应用程序范围之外的资源。这包括但当然不限于:

  web services

  文件系统

  数据库

  RESTful APIs

  通过 localConnection 的其它 Flash 应用程序

Service 封装了这些和外部实体的交互，并管理这个交互产生的 result ，fault 或其它事件.

你可能注意到 Service 和 Model 的基类非常相像。事实上，你可能注意到除了类名，它们其实是一样的。那么为什么用两个类呢? Model 和 Service 类在一个应用程序里有完全不同的职责。这些类的具体实现将不再相像。如果没有这个分离，你将经常发现 Model 类在访问外部服务。这让 Model 有很多职责，访问外部数据，解析结果，处理失败，管理应用程序数据状态，为数据提供一个 API，为外部服务提供一个 API，等等。通过分离这些层有助于缓解这个问题。
![在这里插入图片描述](https://images.cnblogs.com/cnblogs_com/skynet/201203/201203211556134193.png)
<font color="blue">Service 职责

一个 Service 类为你的应用程序提供一个和外部服务交互的 API。一个 service 类将连接外部服务并管理它收到的响应。Service 类通常是无状态的实体。他们并不存储从外部服务收到的数据，而是发送框架事件来让合适的框架 actor 管理响应数据和失败.

<font color="blue">映射一个 Service

有injector 的多个可用的方法可以用来映射你的 Service 类以注入你的其它框架 actor。另外，这些方法也可以用来注入事实上任何类到你的类里。

将一个已存在的实例当做一个单例注入映射，使用下面的语法:

injector.mapValue(MyServiceClass，myServiceClassInstance)

为每个注入映射一个类的新实例，使用下面的语法:

injector.mapClass(MyServiceClass，MyServiceClass)

另外，这也可以用来使用被注入的实现某接口的合适的类来映射这个用来注入的接口.

injector.mapClass(IMyServiceClass，MyServiceClass)

为某个接口或类映射一个单例实例，使用下面的语法：

injector.mapSingleton(MyServiceClass，MyServiceClass)

需要注意重要的一点，当提及上面的一个单例时，它并不是一个单例模式的单例。在这个 Context 之外并不强制它作为一个单例。Injector 简单地确保这个类的唯一一个实例被注入.

>在一个 Service 里监听框架事件
虽然技术上可能，但 强烈不建议 这样做。不要这样做。只是为了说清楚: 不要这样做。如果你这样做了，不要说你没被警告过.

<font color="blue">广播框架事件

Service 类提供一个方便的dispatch方法用来发送框架事件:

dispatch( new ImportantServiceEvent(ImportantServiceEvent.IMPORTANT_SERVICE_EVENT))

<font color="blue">在一个 Service 里解析数据

从外部获取到的数据并不符合我们应用程序的 context。它们是外来者。可以围绕外部数据类型对应用程序进行建模，或者更可取地，转换这些数据以符合应用程序。应用程序里有两处可以进行这项操作/转换。Service 和 Model 都很适合。Service 是进入外部数据的第一个点，所以它是操作一个外部服务返回的数据的更好的选择。外来数据应该在第一个机会转换到应用程序域。

提供一个使用工厂类而不是在 service 里生成应用程序域对象的例子… 适当的

当数据被转换为应用程序域特有的对象之后发出带有强类型负载的事件以被对此关心的 actor 立即使用.

<font color="blue">Service 事件

service 组合的最后一个部分是自定义事件。没有事件的 service 只是哑巴。他们可能做的任何工作都不会被其它框架成员注意到。一个 service 将会使用自定义事件来向应用程序发出声音。事件并不一定是唯一的意图。如果这个 service 正在转换数据它可以使用一个普通的事件来派发强类型的数据给感兴趣的应用程序 actor。


## 5.  扩展点
Robotlegs配合as3signal使用，替代flash原生事件机制。采用as3signal可以有效减少View-Mediator的事件对象，其效率高于事件机制。

下面是flash原生机制与as3signal的性能测试对比（http://alecmce.com/as3/events-and-signals-performance-tests），测试环境：I found the following results on Mac OS X, Flash Player 10.0.42.34, with a release build.

When an event (signal) is dispatched but nothing is listening for it:

![在这里插入图片描述](https://images.cnblogs.com/cnblogs_com/skynet/201203/201203211556141096.jpg)
When an event (signal) is dispatched and handled by one method listener:
![在这里插入图片描述](https://images.cnblogs.com/cnblogs_com/skynet/201203/201203211556148555.jpg)
Robotlegs多模块开发。Robotlegs提供一个多模块工具：robotlegs-utilities-Modular。可以方便的进行模块和模块之间的通信。它看上去十分优雅，用起来也十分方便。下面是几点要注意的地方：

  尽量将模块内事件和模块间的事件分开，不要混杂着用，用于区分它们。

  所有需要公用的对象或类（VO，Model，Service）都在主模块的Startup时注入，这样子模块都可以很方便的用到。

![在这里插入图片描述](https://images.cnblogs.com/cnblogs_com/skynet/201203/201203211556156537.jpg)
在添加大量Sprite到舞台前，可以设置：mediatorMap.enabled=false；。完成后再设置为true，这样可以避免MediatorMap侦听到ADDED_TO_STAGE事件后频繁进行不必要的操作。也可以使用Robotlegs 的 LazyMediator 扩展。

为了游戏中有更好的性能eidiot为 Robotlegs 写了一个 LazyMediator 扩展。（https://github.com/eidiot/robotlegs-utilities-LazyMediator）

如何使用

  在 context 里 override mediatorMap 的 getter 方法:

```csharp
return _mediatorMap || (_mediatorMap = new LazyMediatorMap(contextView, injector));
```
 在view 类的构造函数里增加:
 

```csharp
new LazyMediatorActivator(this);
```
作用

  LazyMediatorMap 不监听显示列表里所有的 ADDED_TO_STAGE 事件而检测所有被添加到显示列表的显示对象。

如何工作

  当 view 被添加到 stage 或从 stage 移除时 LazyMediatorActivator 广播 LazyMediatorEvent。

  LazyMediatorMap 监听 context 的 LazyMediatorEvent 然后检查对应的 view。

## 参考文献

[1]     robotlegs-best-practice（zh）：https://github.com/robotlegs/robotlegs-documentation/blob/master/best-practices-zh-cn.textile#thecontext

[2]     robotlegs-best-practice（en）：https://github.com/robotlegs/robotlegs-framework/wiki/Best-Practices#wiki-thecontext

[3]     ActionScript Developer's Guide to Robotlegs

[4]     Robotlegs 进阶开发笔记，http://www.wersling.com/?p=675

[5]     lash原生机制与as3signal的性能测试对比（http://alecmce.com/as3/events-and-signals-performance-tests）

[6]     多模块开发，http://joelhooks.com/2010/05/02/modular-robotlegs/

[7]     eidiot写的Robotlegs幻灯片，http://eidiot.net/2010/03/28/slides-of-my-robotlegs-presentation-today/

[8]     Flash务实主义——Flash中的MVC，http://uh.9ria.com/space.php?uid=12147&do=blog&id=9102



