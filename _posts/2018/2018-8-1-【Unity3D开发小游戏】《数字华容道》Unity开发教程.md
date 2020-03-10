---
layout: post
category: Unity3D-Game
title: 【Unity3D开发小游戏】《数字华容道》Unity开发教程
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
因为最强大脑让【数字华容道】这款老游戏又火了一把，那就让我们来看看这个游戏是怎么实现的吧，有什么样的算法呢。

## 二、GitHub

GitHub地址：https://github.com/764424567/Game_NumberHuarongRoad
*注意：可以直接在GitHub仓库克隆或者下载源代码

## 三、正文
### 【数字华容道】
### 一：核心


----------


寒假的时候看最强大脑。
有些项目看的我也想试试，就在豌豆荚上搜索数字华容道，找到一个下载安装后，发现面丑就算了，自适应没有做好，玩不了就删掉了。
想了想这个游戏的逻辑，好像很简单，那自己做一个吧。
如此我挖了我考研期间的第一个坑。


----------
核心玩法的逻辑图
![这里写图片描述](https://img-blog.csdn.net/20180318215153849?watermark/2/text/Ly9ibG9nLmNzZG4ubmV0L3FxXzM0Mjg4MTg2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
我猜看到这图，各位看官可能想退出页面了。。。
我也是第一次画这东西，非常简陋，看不下去的直接跳过看下面的吧。


----------
刚开始琢磨做点小游戏的时候，有些[数]，先放在前面跟大家分享一下：
在不知道能不能做好之前，暂时不管美术，先把一个原型做出来，后面慢慢画美术。
在没有雏形之前，不要着急想着如何丰富这个游戏，先做最基本款，后面有精力了再优化更新。
心里有了这两点[数]，能更不容易半途而废。


----------
## 二、基本画面
1. 新建项目
1. 去ps里弄数字1-15的图片和一张空白的。要是有其他的素材也行。这个随便一个画图软件就可以弄，分享一组博主自己做的图片，有点丑。。。凑合着用
![1.jpg](https://img-blog.csdn.net/20180801174112672?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)![2.jpg](https://img-blog.csdn.net/20180801174118672?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)![3.jpg](https://img-blog.csdn.net/20180801174133993?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)![4.jpg](https://img-blog.csdn.net/20180801174205930?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![5.jpg](https://img-blog.csdn.net/20180801174218613?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)![6.jpg](https://img-blog.csdn.net/20180801174232390?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)![7.jpg](https://img-blog.csdn.net/20180801174246535?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)![8.jpg](https://img-blog.csdn.net/2018080117425871?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![9.jpg](https://img-blog.csdn.net/20180801174315258?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)![10.jpg](https://img-blog.csdn.net/20180801174326600?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)![11.jpg](https://img-blog.csdn.net/20180801174340572?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)![12.jpg](https://img-blog.csdn.net/2018080117435163?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![13.jpg](https://img-blog.csdn.net/20180801174403336?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)![14.jpg](https://img-blog.csdn.net/20180801174414148?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)![15.jpg](https://img-blog.csdn.net/20180801174424381?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)![0.jpg](https://img-blog.csdn.net/20180801174106505?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
2. 在canvas下新建创建一个Panel，在Panel下新建一个空的物体，命名为table。然后给他添加一个Grid Layout Group组件（能让子物体成网格式分布），设置一下参数，然后在table下添加image，ctrl+d快速复制一共16个。然后看着调整一下Grid Layout Group 的参数。
![这里写图片描述](https://img-blog.csdn.net/20180802175812757?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
3. 在rect transform 组件中设置自适应，点一下那个图标，按住alt键，选择中间那个。用unity做自适应挺简单的，后面再有自适应就不提了。
![这里写图片描述](https://img-blog.csdn.net/20180802174552197?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
5. 把table下的image改名叫piece1-piece16了，把图片资源一个个对应拖到image上的source image上
![这里写图片描述](https://img-blog.csdn.net/20180802175218945?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
6. 建了一个button来表示开始。
![这里写图片描述](https://img-blog.csdn.net/20180318215302757?watermark/2/text/Ly9ibG9nLmNzZG4ubmV0L3FxXzM0Mjg4MTg2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


### 三、GameManager
写代码了
1.创建一个scripts文件夹，在里面创建一个c#脚本文件叫GameManager，打开
2.先写个单例吧，整个项目只能有这一个脚本在运作。记得之后要挂载在一个gameobject上。我挂到table上了。

```csharp
#region 单例 
public static GameManager Instance 
{ 
	get 
	{ 
		return instance; 
	} 	 
	private static GameManager instance = null; 
	void Awake() 
	{ 
		if (instance == null) 
		{ 
			instance = this; 
		} 
		else if (instance != this) 
		{ 
			DestroyImmediate(this);//立即销毁 
		} 
	}
} 
#endregion
```

3.打算是有两个数组，一个数组是1-16排好序的，另一个是1-16乱序的数组，只要把乱序数组调整回有序的数组，我们就赢了。也就是说那个有序数组是为了方便比较才创建的。

```csharp
private int[] Array; 
private int[] Order; 
void Start () 
{
	 Array = new int[] { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16 };
	 Order = Array; 
}
```

4.接下来是要打乱数组里的数字顺序，我采取的策略是随机两两互换。我想了一会，也就想到这个比较好，还挺有意思。不知道大家有没有更好的方法。

```csharp
/// <summary> 
/// 数组里的数字随机两两交换 
/// </summary> 
/// <param name="a">数组</param> 
/// <param name="temptimes">交换次数</param> 
void A_random(int[] a,int temptimes) 
{
	for(int i = 0; i < temptimes; i++)
	{ 
		int r = Random.Range(0, a.Length); 
		int temp = a[i];  
		a[i] = a[r];  
	}
	foreach (int num in a)
	{ 
		Debug.Log(num);  
	} 
}
```

5.上面那个函数只是动数据，没有动画面。接下来就是写函数改变图块位置，使画面与数据一致。
这里面有一个对应关系可以利用，在unity面板里，table子物体的索引值=乱序数组array的索引值。

```csharp
/// <summary> 
/// 图块按照数组重新排序 
/// </summary> 
/// <param name="gameObject">table</param> 
/// <param name="a">乱序数组</param> 
void P_sequence(GameObject gameObject, int[] a) 
{ 
	foreach(int num in a) 
	{ 
		string name = "piece" + num; 
		gameObject.transform.Find(name).SetSiblingIndex(-1); 
	} 
}
```
6.那接下来，我们的开始游戏button对应的函数，可以这样写。别忘了在unity界面，把开始游戏button和这个函数绑定上。我们还要获取table这个玩意的gameobject，在unity界面也要记得绑定上，即给代码里的“table”变量赋值。

```csharp
public GameObject table; 
public void GameStart() 
{ 
	A_random(Array, 16); 
	P_sequence(table, Array); 
}
```

7.接下来就是交换了，逻辑很简单：点击一个图块，如果图块周围有白板，那被点击的图块和白板交换位置。关键点就是怎么检测图块周围有白板。这是这个游戏核心功能实现的最关键的地方。我本来是想【找被点击的x周围有没有白板】，后来想了想，觉得【找白板周围的数字是不是x】会简单一点。
亲爱的看官可以画图思考一下，然后看一下我的if（条件句）里的条件。（我好懒哦，不想在这详细说明了，因为我还没有吃饭）如果有更好的方法，请赐教，也欢迎讨论。

```csharp
/// <summary> 
/// 判断【数字16（白板）】上下左右的项的索引值，如果是被点击的x，则交换两个索引值里的数据，然后调用函数刷新面板里图块的顺序 
/// </summary> 
/// <param name="x">点击图块的索引值</param> 
void A_exchange(int x) 
{ 
	int index=table.transform.Find("piece16").GetSiblingIndex()+1; 
	if (((index == x - 1)&&!(index%4==0))|| ((index == x + 1) && !(index % 4 == 1))|| (index + 4 == x)|| (index - 4 == x)) 
	{  
		int temp = Array[x-1];   
		Array[x-1] = Array[index-1];  
		Array[index-1] = temp; 
		P_sequence(table, Array); 
	} 
	if (Array == Order) 
	{ 
		//成功  
	}
}
```

8.下来是给每个图块添加一个button组件，在onclick里绑定如下函数，传递的参数对应为图块本身的数字。这样在乱序的时候，点击一个图块时，我们先获取这个图块对应的数组索引值/table子物体索引值，然后调用并传递给A_exchange函数，让其从索引值判断上下左右的关系，从而判断是否可以交换位置。

```csharp
/// <summary> 
/// 获取数字num对应Array的索引值和table子物体的索引值 
/// </summary> 
/// <param name="num"></param> 
public void Getsiblingindex(int num) 
{ 
	int s=table.transform.Find("piece" + num).GetSiblingIndex(); 
	A_exchange(s + 1); 
}
```

### 四：运行
![这里写图片描述](https://img-blog.csdn.net/20180318215332461?watermark/2/text/Ly9ibG9nLmNzZG4ubmV0L3FxXzM0Mjg4MTg2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
1. 大概就是这个样子了，很简陋，有非常多可以改进的地方。
2. 因为要准备考研，所以不一定能改进成什么样，但是要是有时间了一定会再改进的。
3. 留一些待改进的方向，然后要去觅食了。
4. 留了个panel没处理，我想让没按”开始游戏“的时候，table是不可见的。
5. 计时器没做。要是还有时间可以做一个本地的得分排名。
6. 整个都写在gamemanager脚本里了，代码不够简洁清晰。
7. 这个数字字体我好喜欢，但整个画面不好看，美术上可以做不少工作，还可以加动画。
8. 当然是打包成apk了。


### 五、源码
已经上传到CSDN
https://download.csdn.net/download/q764424567/11229226
