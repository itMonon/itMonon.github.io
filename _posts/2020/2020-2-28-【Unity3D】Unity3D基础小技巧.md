---
layout: post
category: Unity3D-Daily
title: 【Unity3D】Unity3D基础小技巧
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
掌握一些Unity编辑器的小技巧，会让你开发以及使用更加快捷有效。这篇文章就分享一些Unity在使用使用的一些小技巧，有什么不对的记得留言哦。

## 二、原文地址
原文出处：蛮牛
原文作者：蛮牛教育讲师 邵伟老师
原文链接：http://www.manew.com/thread-143268-1-1.html

## 三、正文
### 1.高亮选择
在Scene面板右上角的Gizmo下拉列表中，可以通过设置Selection Outline选项决定是否在选中物体时显示边缘高亮的标识。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/142532ag2kg816g2kck2km.jpg.thumb.jpg)
### 2.Pixel Perfect Camera
在摄像机上挂载Pixel Perfect Camera组件能够使2D像素风格的游戏画面更加整洁清晰。此组件需要使用Package Manager安装2D Pixel Perfect包。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/142551tqjgqbt5b3b4zqqz.jpg.thumb.jpg)
### 3.以Y轴为依据进行Sprite排序
对于2D游戏，将Transparency Sort Mode (Edit > Project Settings > Graphics ) 设置为Custom Axis，然后设置Transparency Sort Axis，场景中的Sprite可以根据Y轴进行排序。如下图所示，当设置为（0,1,0）时，Y坐标相对较大的Sprite排在Y坐标相对较小的Sprite之下，当设置为(0,-1,0)时，则相反。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/142614qbbea1emzmwqgm1m.jpg.thumb.jpg)
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/142652ta2jnuubasowppak.jpg.thumb.jpg)
### 4. 延迟销毁游戏对象
默认情况下，使用Destroy()方法会立即销毁游戏对象，如果想延迟一段时间再销毁，可在此方法中传递一个时间参数，如下：
```csharp
Destroy(gameObject,2f);
```
### 5.快速新建基于自定义Shader的材质
在Project面板中选中一个自定义Shader，右键选择新建材质（Create>Material），则材质默认使用的着色器为z之前选择的Shader，同时材质名称为Shader的名称。

### 6.脚本不挂载到游戏对象执行
通常情况下，新建的脚本要挂载到游戏对象上才能运行，如果在脚本中的方法前使用[RuntimeInitializeOnLoadMethod(RuntimeInitializeLoadType.AfterSceneLoad)]，可以不用挂载到任何游戏对象上即可在程序运行时执行此方法，方便在在程序初始化前做一些额外的初始化工作。如下代码所示：
```csharp
[RuntimeInitializeOnLoadMethod(RuntimeInitializeLoadType.AfterSceneLoad)]
public static void DoSomething()
{
    Debug.Log("It's the start of the game");
}
```

### 7.保存程序运行时组件属性的改变
在程序运行时改变组件的各属性值，当停止运行后，这些改变将重置为编辑状态下的数值，。程序运行时改变了组件的属性值，可以点击组件右上角的齿轮按钮，选择Copy Component命令，停止播放后，在相同的组件上，执行Paste Component Value，从而能够保存在运行时对该组件做出的改变。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/143543paeeai0szwz9avwi.jpg.thumb.jpg)

### 8. 获取一个随机布尔值
我们知道Random.value能够返回0~1之间的随机数，所以让此随机数与0.5f进行比较，就能够获取一个随机的布尔值True或者False，如下代码所示：
```csharp
bool trueOrFalse = (Random.value > 0.5f);
```

### 9. 使用Struct代替Class
如果数据结构仅保存了有限的几个数值变量，可以考虑使用struct代替Class，因为Class实例由垃圾回收机制来保证内存的回收处理;而struct变量使用完后立即自动解除内存分配。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/143618liz5lvk7ifk7kjs7.jpg.thumb.jpg)
### 10. Visual Studio 自动语句补全 ![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/143650svvbqvnbpqk2vezz.jpg.thumb.jpg)
当使用Visual Studio进行代码编写时，可双击Tab键来辅助完成像if、for、switch等语句的补全。

### 11.协程嵌套
在一个协程里开启另外一个协程，可使用以下方法：
```csharp
void Start()
{
    StartCoroutine(FirstCo());
}
IEnumerator FirstCo()
{
    yield return StartCoroutine(SecondCo());
}
IEnumerator SecondCo()
{
    yield return 0;
}
```

### 12.脚本变量参与动画制作
使用工具还可以改变脚本的变量。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/143736fe4s6imoo8eigiu6.jpg.thumb.jpg)
### 13/14. Animation窗口快捷键
在Animation窗口中，按下Ctrl+A，所有关键帧将集中显示在窗口中；选择某些关键帧，按下F键，可将它们居中显示在窗口中；按下C键，可以在曲线视图和关键帧视图间切换；按下K键添加关键帧。

### 15.反向播放动画
在Animator窗口中，设置动画的Speed属性为-1可使动画片段反向播放。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/145029h9i7kq4p95ifmg7n.jpg.thumb.jpg)
### 16.快速比较距离
将两点之间的距离与一个固定距离进行比较时，可使两点相减然后取平方（即sqrMagnitude），然后用该值与某个距离值的平方进行比较。不建议使用Vector3.Distance方法获取两点之间距离，然后与给定的距离值进行比较。因为Vector3.Distance(a,b) 相当于 (a-b).magnitude，即求平方后开根，而sqrMagnitude方法省去了求平方根的操作，所以比magnitude执行快。
建议：
```csharp
if ((pointA - pointB).sqrMagnitude < dist * dist)
{
}
```
不建议：

```csharp
if (Vector3.Distance(pointA, pointB) < dist)
{
}
```
### 17.使用TextMeshPro
使用TextMeshPro能够获得更多的文字控制自由度，并且能够有效防止文字边缘模糊。如下图所示，第一行文字通过"Create > UI > Text"命令创建，第二行文字通过"Create > UI > TextMeshPro - Text"命令创建。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/150011ivqsfvusc8nvucpa.jpg.thumb.jpg)
### 18.在Inspector面板中显示私有变量
将私有变量标记为SerializeField，可在Inspector面板中将其显示。
```csharp
[SerializeField]
private int myNumber = 20;
```
### 19. 在Inspector面板中隐藏公有变量
如果不希望在Inspector面板中显示公有变量，可将其标记为[HideInInspector]。
```csharp
public int myNumber = 20;
```
### 20. 变量重命名后继续保持值
当变量重命名后，如果希望继续保留其数值，可使用FormerlySerializedAs，如下代码所示：
```csharp
[FormerlySerializedAs("hp")]
public int myNumber = 20;
```
需要引用命名空间：

```csharp
using UnityEngine.Serialization;
```
### 21.使用文件夹快捷方式
可将经常访问的文件夹的快捷方式拖入Project面板中，双击快捷方式可快速打开此目录。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/150338u8rweojymjjj85ml.jpg.thumb.jpg)
### 22/23.F与Shift+F
选择游戏对象，按下F键，可将Scene的视口中央移动到该游戏对象处；按下Shift+F，可将视口与该游戏对象锁定，即无论如何移动游戏对象，视口中央始终跟随此游戏对象。

### 24.对齐Scene与Game视图
在Hierarchy面板中选择摄像机，按下Ctrl+Shift+F，可将摄像机移动到能够呈现Scene窗口中内容的位置。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/150409n9uwivkdybvhbhik.jpg.thumb.jpg)
 
### 25. CompareTag方法
当对游戏对象的Tag进行比对时，从性能考虑，可使用CompareTag方法，不建议使用双等号进行判断。
建议：
```csharp
if (gameObject.CompareTag("Enemy"))
{
}
```
不建议：
```csharp
if (gameObject.tag == "Enemy")
{
}
```

### 26.使用空游戏对象作为分隔符
在Hierarchy面板中，可以使用名称中带有分隔符的空游戏对象进行组织管理。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/153523bzs5snhlkdzyhhpx.jpg.thumb.jpg)

### 27. 查找含有某组件的游戏对象
如果需要查找挂载了某个组件的游戏对象，直接在Hierarchy面板的搜索框中输入组件名称即可，需要注意组件名称中的空格，比如搜索”MeshCollider“而不是”Mesh Collider“。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/153542miz6ovo3ybzhy6b3.jpg.thumb.jpg)

### 28. 查找某种类型的资源
在Project面板中的搜索框中输入"t:"+资源类型，可以过滤显示某种类型的资源，比如输入"t:scene"，会过滤出所有场景文件，输入"t:texture"，则会显示所有贴图。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/153846fg2apaa83pjx5a9j.jpg.thumb.jpg)

### 29. 移动代码行
在Visual Studio中，使用快捷键Alt+上下键，可以在代码块中快速上移/下移光标所在的代码行，不用复制粘贴。

### 30.快速查看组件文档
点击组件右上角的文档按钮，可快速打开关于当前组件的文档。建议下载离线文档，以便更加快速打开文档，如果没有下载，Unity将打开在线文档。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/155754rbra2rjg3ddr0a0v.jpg.thumb.jpg) 

### 31.文档版本历史
在Unity文档中点击Documentation Version链接，可查看不同版本的文档。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/155911mp7ppfz0fiwvmts7.jpg.thumb.jpg)
### 32. 展开/折叠所有节点
在Hierarchy面板中，按下Alt键，鼠标左键点击树形节点，可展开/折叠当前节点下的所有子节点。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/155958e8ep8s17e1dy9sos.jpg.thumb.jpg)
### 33.保存编辑器窗口布局
Unity窗口布局可自定义，调整完毕以后，如果希望以后继续沿用此布局，点击编辑器右上角的Layout下拉列表，选择命令Save Layout，可将当前窗口布局进行保存。



### 34+35. 改变编辑器颜色
选择命令Editor > Preferences命令，可自定义编辑器当前主题的颜色。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/160023ei520ohvg10ee8r3.jpg.thumb.jpg)
修改Playmode tint的颜色值，可以改变编辑器在运行模式时的颜色，以提醒开发者此时为运行模式。


### 36.开关场景特效
在Scene面板顶部的图片下拉列表中，可选择开关某种类型的特效。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/160044d07tp7d2awa0gpf1.jpg.thumb.jpg)
### 37.MenuItem属性
要在编辑器的菜单栏中选择执行编写的函数，可在函数前添加MenuItem属性，如下代码所示：
```csharp
[MenuItem("MyMenu/Do Something")]
static void DoSomething()
{
}
```
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/160114w10zen44j223nym2.jpg.thumb.jpg)

### 38.ContextMenu
使用ContextMenu属性标记函数，能够在脚本所在的上下文菜单中调用，如下代码所示：
```csharp
[ContextMenu("Do Something")]
void DoSomething()
{     
}
```
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/160223spc3pxrq6xsrqpfw.jpg.thumb.jpg)

### 39+40. 隐藏和锁定层
在编辑器右上角的Layers下拉列表中，点击对应层右侧的眼睛按钮，可以隐藏或显示某个层上的对象；点击锁按钮，可对某个层进行锁定或解锁，当被锁定后，该层上的所有对象将不能被选择。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/160304qe1o1xwrvrgonts6.jpg.thumb.jpg)

### 41.层子菜单
当创建层时，使用斜杠符进行路径式命名可以为层添加子菜单，可以更好地组织项目。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/160336nzdbjkyygjbgjjrd.jpg.thumb.jpg)
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/160355v22mifyy3iaw9jn2.jpg.thumb.jpg)

### 42. 使用Scripting Define Symbols定义脚本
在不同的目标平台下添加 Scripting Define Symbols（Project Settings > Player > Scripting Define Symbols），以分号分隔，可以将这些符号像使用Unity内置标签一样用作#if指令的条件。


### 43+44.颜色
在使用Color控件的滴管工具进行颜色选择时，可以拾取Unity编辑器之外的颜色。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/160444xaaemg6zz5a6qwag.jpg.thumb.jpg)
在颜色属性之间也可以使用右键命令进行复制粘贴。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/160521cgigkgb5isgfzz21.jpg.thumb.jpg)
 
### 45.最大化窗口
使用快捷键Shift+空格键，可以快速最大化鼠标所在的窗口，而不用选择窗口右上角的Maximize命令


### 46. 序列化Struct和Class
在数据类型Struct和Class声明前使用[System.Serializable]，可以将其显示在Inspector面板中进行赋值。


### 47.碰撞矩阵
在 Edit > Project Settings... > Physics 中，通过设置Layer Collision Matrix 可以决定能够互相碰撞的层。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/160552d33nyc8731d1vh0z.jpg.thumb.jpg)

 



### 48.Collider相互作用矩阵
当两个对象发生碰撞时，会发送不同的碰撞事件，如OnTriggerEnter、OnCollisionEnter等等，这取决于具体的碰撞体设置，下表列出了不同类型的碰撞体发生碰撞时所能发出的事件类型。详情可参考Unity 官方文档：https://docs.unity3d.com/Manual/CollidersOverview.html
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/160552d33nyc8731d1vh0z.jpg.thumb.jpg)

 



### 49. 数值输入
在Inspector面板中涉及到数值输入的字段，不仅可以直接输入数据，还可以在输入框中输入数学表达式，按下回车后Unity会将计算结果填充到输入框中。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/160644g4fhif8xan1affrx.jpg.thumb.jpg)

 

### 50.锁定Inspector
点击Inspector右上角的锁定按钮，或在上下文菜单中选择Lock命令，可以将当前选中游戏对象的Inspector面板锁定。然后选择Add Tab > Inspector命令，添加一个Inspector，这样能够方便在两个游戏对象之间互相拷贝组件数据。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/160715ehy3dondcz3reo0d.jpg.thumb.jpg)
 



### 51.Inspector调试模式
在Inspector面板右上角的下拉菜单中，选择Debug命令，启动调试模式，此时将显示组件包含的所有变量，包括私有变量，当运行编辑器时，可以实时查看各组件所有变量的变化。


### 52.高亮显示Debug.Log对应的游戏对象
当使用Debug.Log方法输出信息时，可将gameObject作为此方法的第二个参数，当程序运行时，点击Console面板中对应的输出信息，可在Hierarchy面板中高亮显示挂载了此脚本的游戏对象。



```csharp
void Start()
{
    Debug.Log("this is a message",gameObject);
}
```

![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/160753kmr7pybmfsb47am7.jpg.thumb.jpg)
 



### 53.风格化Debug.Log的输出信息
当Debug.Log方法的输出消息是字符串时，可以使用富文本标记来强调内容。如下代码所示：

```csharp
Debug.Log("<color=red>Fatal error:</color> AssetBundle not found");
```

输出效果：
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/160823pbffw26bkzfbkfvk.jpg.thumb.jpg)

 



### 54.绘制调试数据
当变量随着时间的推进而改变时，可使用AnimationCurve实例在程序运行时绘制此数据，如下代码所示：



```csharp
public AnimationCurve plot = new AnimationCurve();
void Update()
{
    float value = Mathf.Sin(Time.time);
    plot.AddKey(Time.realtimeSinceStartup, value);
}
```




返回Unity编辑器，运行程序，点击plot属性，此时会随着时间动态绘制数据的变化情况，如下图所示：

![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/160859xwwojis79qx9bqsm.jpg.thumb.jpg)
 



### 55.快速新建脚本并挂载到游戏对象上
选择游戏对象，在Inspector面板上点击Add Component按钮，在搜索框中输入新建的脚本名称并回车，可新建脚本并挂载到目标游戏对象上，双击脚本名称进行脚本编写。


### 56.导入第三方项目文件
Unity能够读取部分第三方创作工具保存的项目文件，比如Photoshop的PSD，Blender的源文件等，不需要从这些软件导出中转文件格式，比如Jpg、FBX等。


### 57.导入后保留PSD文件的图层结构
将PSD文件另存为PSB格式，将其导入Unity后可保留文件图层结构，此时需要在Package Manager中安装2D PSD Importer，并且在文件的导入属性中设置Texture Type 为Sprite (2D and UI)。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/160928np57sw9hp8zq8siu.jpg.thumb.jpg)

  


### 58+59.为游戏对象指定/自定义图标
点击游戏对象Inspector面板左上角的下拉菜单，可为游戏对象指定一个特定颜色的标识，这对空游戏对象的可视化也比较有用。点击Other...按钮，可以用自己的图片来进行标识。
  ![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/161001u9c488sp91aarscw.jpg.thumb.jpg)


### 60/61.显示/隐藏Gizmo
点击Scene面板右上角的Gizmo下拉列表，可以选择显示或隐藏某类组件的图标和Gizmo标识；也可点击Game面板右上角的Gizmo按钮，显示或隐藏所有资源的图标和Gizmo。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/161025j747zhutt5995v7m.jpg.thumb.jpg)

 



### 62.字符串拼接
可使用StringBuilder进行字符串的拼接，不要使用字符串相加的形式，因为这样会带来额外的内存垃圾。如下代码所示：


```csharp
StringBuilder myStr = new StringBuilder();
myStr.Append("Hello").Append("The").Append("World");
```




不建议：


```csharp
string myStr = "Hello" + "the" + "world";
```

使用StringBuilder需要引用命名空间System.Text。


### 63.使用ScriptableObjects管理游戏数据
对于游戏数据比如武器、成就等，可使用ScriptableObjects在编辑器中进行有效组织。如下代码所示：

```csharp
using UnityEngine;
public class NewBehaviourScript : ScriptableObject
{
    public string ItemName;
    public int ItemLevel;
    public Texture2D ItemIcon;
}
```

![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/161200az0uuk043kz737yx.jpg.thumb.jpg)



 



### 64.编辑器播放时修改脚本后的处理
选择 Edit > Preferences > General 命令，在Script Changes While Playing中，可以设置编辑器在播放状态下如果脚本发生改变后的处理，比如停止播放重新编译等。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/161227srvsgjceqsjc69f4.jpg.thumb.jpg)

 



### 65.自定义窗口
将类继承自EditorWindow，可以添加自定义窗口，在此基础上编写一些命令和工具，如下代码所示：

```csharp
using UnityEngine;
using UnityEditor;
 
public class ExampleWindow : EditorWindow
{
    [MenuItem("Window/Example")]
    public static void ShowWindow()
    {
        GetWindow<ExampleWindow>("Example");
    }
}
```




执行效果：
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/161305yqmom79mukfuouo6.jpg.thumb.jpg)
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/161330dnnhuhu5h5nsud5q.jpg.thumb.jpg)

 

 



### 66.自定义Inspector
也可对Inspector进行自定义，添加一些控件。如下代码所示：

```csharp
using UnityEngine;
using UnityEditor;
 
[CustomEditor(typeof(Sphere))]
public class SphereEditor : Editor
{
    public override void OnInspectorGUI()
    {
        GUILayout.Label("自定义Inspector");
        GUILayout.Button("确定");
    }
}
```




执行效果：
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/161405gtrl8c1jcdt5tvqt.jpg.thumb.jpg)
 


### 67.工具快捷键
使用快捷键Q、W、E、R、T、Y切换移动、旋转、缩放等工具。

### 68.使用RectTransform工具缩放3D物体
RectTransform工具一般用于缩放2D物体，对3D物体使用该工具可以在某个二维平面对其进行缩放，这取决于物体与视口的关系。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/161426bh7svphm6ukmqmdc.jpg.thumb.jpg)

 

### 69+70+71.吸附
按下Ctrl键对游戏对象进行移动、旋转、缩放，将以步进的形式进行操作，选择Editor > Snap Settings...命令，可设置步进大小。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/161448tqdlhoq1421y9z11.jpg.thumb.jpg)

 



按下V键，在游戏对象上选择顶点进行拖放，将以此顶点为基础，把游戏对象吸附到其它顶点的位置。


### 72. 管理程序集
在Project面板中选择Create > Assembly Definition 命令，创建程序集文件，然后将其拖放到指定的文件夹中，定义脚本依赖关系，可以确保脚本更改后，只会重新生成必需的程序集，从而减少编译时间。


### 73.WaitForSecondsRealtime
当时间缩放为0时（即Time.timeScale=0f），waitForSeconds方法将不会停止等待，后续代码也不会执行，此时可使用WaitForSecondsRealtime方法，如下代码所示：

```csharp
Time.timeScale = 0f;
yield return new WaitForSecondsRealtime(1f);
```




### 74+75.缓存组件引用
当某组件需要被频繁访问时，可在初始化时预先获取该组件的引用，从而避免在访问时由于重复获取引起的性能开销。


```csharp
private Rigidbody rb;
 
void Start()
{
    rb = GetComponent<Rigidbody>();
}
 
void Update()
{
    rb.AddForce(0f, -2f, 0f);
}
```

同样的情况，也不要在使用Camera.main获取摄像机组件，尤其避免使用类似以下方法：

 

```csharp
Camera cam = GameObject.FindGameObjectWithTag("MainCamera").GetComponent<Camera>();
```

这样会带来更大的性能消耗。


### 76.字符串性能优化
如果某字符串在整个应用过程中不会改变且被频繁使用，可将其存储在静态只读变量中，从而节省内存分配，如下代码所示：

```csharp
static readonly string Fire1 = "Fire1";
 
void Update()
{
    Input.GetAxis(Fire1);
}
```

不建议：

```csharp
void Update()
{
    Input.GetAxis("Fire1");
}
```




### 77/78/79/80.方便使用的元数据
为变量添加一些属性可使它们在Inspector面板中更容易被使用。在变量前添加Range属性可将其限定在某个范围内使用滑块进行调节，如下代码所示：

```csharp
[Range(0f,10f)]
public float speed = 1f;
```




执行效果：
 ![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/161620rxz7ogq1oyxos418.jpg.thumb.jpg)

两个变量声明之间加入[Space]可在Inspector中添加一个空行；添加Header可在Inspector面板中加入一段文字，如下代码所示：

```csharp
public float speed = 1f;
public int size = 10;
```


执行效果：
 ![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/161647q8e8inx303nsnz8a.jpg.thumb.jpg)



在变量前加入Tooltip，当鼠标悬停在Inspector面板中的变量上时，可显示关于此变量的说明，如下代码所示：

```csharp
[Tooltip("移动速度")]
public float speed = 1f;
```

执行效果：
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/161711gx31xkucrcechkw7.jpg.thumb.jpg)
 



### 81.在Unit编辑器中访问Asset Store
Asset Store可在Unity编辑器和网页浏览器中访问。（此条有充数嫌疑）


### 82.合并场景
在Project面板中，将一个场景文件拖到另外一个上，可将场景进行合并。


### 83/84.创建游戏对象/数组元素副本快捷键
选择一个游戏对象，使用快捷键Ctrl+D可快速创建该游戏对象的副本，同样的方法可创建数组元素的副本。


### 85.组件预设
当完成某个组件的属性设置后，可点击组件右上角的预设按钮，将当前属性设置保存为预设，方便后续进行组件设置时使用。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/161736hd1fd0wq0bqrdv6v.jpg.thumb.jpg)

 

### 86.遍历游戏对象所有子物体
可使用foreach循环遍历游戏对象的所有子物体，如下代码所示：

```csharp
foreach (Transform child in transform)
{
    Debug.Log(child.name);
}
```




### 87.通过脚本改变游戏对象在Hierarchy中的顺序
使用transform.SetSiblingIndex方法可以设置游戏对象在Hierarchy面板中的顺序，如下代码所示：

 

```csharp
transform.SetSiblingIndex(1);
```

以上代码实现在游戏运行时，设置游戏对象在Hierarchy面板中的顺序为同级节点中的第二个。


### 88.保存选择状态
当选择了多个游戏对象后，可在 Edit > Selection 的子菜单中选择一个Save Selection项，暂存当前选择状态。选择Load Selection+对应的序号，即可恢复某个选择状态。此方法对跨节点选择多个对象的情况非常适用，这样将不必依次展开节点重新进行查找选择。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/161819jtvzw62w9xz5de47.jpg.thumb.jpg)
 


### 89.#region 和 #endregion
使用#region 和 #endregion可以将两者之间包含的代码折叠，方便阅读。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/161840hr12orw55q2wa57q.jpg.thumb.jpg)
  

### 90.通过脚本暂定编辑器播放
使用EditorApplication.isPaused可通过代码在编辑器播放时将其暂停，如下代码所示：

```csharp
void Update()
{
    if (Time.time >= 10f)
    {
        EditorApplication.isPaused = true;
    }
}
```


需要引用命名空间UnityEditor。

### 91.逐帧查看程序运行
点击暂停按钮右侧的步进（Step）按钮，可以在程序运行时逐帧查看程序运行状态。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/161903oggzgxzg5fj7jlwz.jpg.thumb.jpg)
 




### 92/93/94.查看游戏性能统计
点击Game窗口右上角的Stats按钮可以查看游戏性能统计数据，如帧率、批处理等指标。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/161925y3djqz0sicl3cxy2.jpg.thumb.jpg)
 



查看更加详细的分析数据，可使用Window > Analysis > Profiler工具；使用Profiler.BeginSample和Profiler.EndSample方法可在Profiler中查看函数的资源使用情况，如下代码所示：

```csharp
Profiler.BeginSample("expensive");
CalculateSomething();
Profiler.EndSample();

```



需要引入UnityEngine.Profiling命名空间。
 ![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/161955clfpcb6v2lt2tzq6.jpg.thumb.jpg)



### 95.弹出预览窗口
通常情况下，项目资源在Inspector面板底部均有一个预览窗口。鼠标右键点击预览窗口顶部，可将该窗口弹出，作为独立窗口，放置在编辑器的任意位置。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/162016qln9n8tzz9te88m9.jpg.thumb.jpg)

 



### 96.测试游戏时静音
点击Game窗口右上角的Mute Audio按钮，可在编辑器播放时将所有声音关闭。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/162038bdui3jetittyuvff.jpg.thumb.jpg)
 



### 97.InvokeRepeating方法
InvokeRepeating能够按照一定的时间间隔反复执行某个函数，若不使用CancelInvoke方法，InvokeRepeating将持续执行，即使将方法所在的脚本关闭。


### 98.Frame Debugger
使用Frame Debugger（Window > Analysis > Frame Debugger）可以查看每一帧的渲染过程。


### 99.Physics Debugger
使用Physics Debugger（Window > Analysis > Physics Debugger）可以查看碰撞引起的异常，当开启Collision Geometry选项后，场景中所有游戏对象的碰撞体都将被绘制出来，而不用依次选择游戏对象进行检查。如下图所示，球体因为添加了不正确的Box Collider，在物理碰撞时必然不能达到预期的表现效果。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/162100ne3fx66get6bensn.jpg.thumb.jpg)

 



### 100.不要做MMORPG游戏！
这应该是作者被MMORPG长期折磨的结果，此处纯搞笑。
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/142635ie7y8ztyedyeoyw2.jpg.thumb.jpg)
![在这里插入图片描述](http://img.manew.com/data/attachment/forum/201901/21/150758lwj8weowetowogvf.jpg.thumb.jpg)
