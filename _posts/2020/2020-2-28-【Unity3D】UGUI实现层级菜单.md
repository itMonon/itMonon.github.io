---
layout: post
category: Unity3D-Daily
title: 【Unity3D】UGUI实现层级菜单
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
层级菜单在Unity中用到的并不多，主要是做分类的时候用的比较多，今天就给大家分享几个层级代码，扩充一下，写成插件也是不错的。

首先看一下效果吧：
1.
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830161902402.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
2.
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830161942817.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
3.
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830161959516.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019083016200749.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
4.
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830175602568.png)
5.
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830162435766.png)


## 二、资源下载
源文件：
https://download.csdn.net/download/q764424567/11644500
## 三、正文
<ul>
<li>
<p><a href="#1" rel="nofollow" rel="nofollow" data-token="a395fa12be1d1b4ad53a47bdfe2162fa">【Unity3D】UGUI实现层级菜单</a></p>
<ul>
<li>
<p><a href="#1.1" rel="nofollow" rel="nofollow" data-token="cf6d9fd3331020e3c4e62d5637cf750e">第一种实现效果</a></p>
</li>
<li>
<p><a href="#1.2" rel="nofollow" rel="nofollow" data-token="dbb8bc0cb8b3aa5267c7089c6eb54b9e">第二种实现效果</a></p>
</li>
<li>
<p><a href="#1.3" rel="nofollow" rel="nofollow" data-token="f55ed47708a13d4c1f56594e7b5ea6f2">第三种实现效果</a></p>
</li>
<li>
<p><a href="#1.4" rel="nofollow" rel="nofollow" data-token="d22fabdd1ec2b72e66c7cbba420ff4b4">第四种实现效果</a></p>
</li>
<li>
<p><a href="#1.5" rel="nofollow" rel="nofollow" data-token="1f6a31be7c8f40e17f37bd7981cdda98">第五种实现效果</a></p>
</li>
</ul>
</li>
</ul>

<h1 id="1.1">

### 第一种实现效果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830161902402.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
实现原理：这个是用系统自带的UGUI  Scroll View组件，脚本控制创建父物体，父物体身上挂载有初始化子物体的脚本
优缺点：
优点是实现简单，不需要多与的插件，代码量也不大，控制比较方便
缺点是只能实现两个层级的显示
实现过程：
1、新建一个Scrpll View
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830163556542.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
2、制作预制体
界面就这么设计就行：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830164915743.png)

名字改一下
Content：父节点容器
ParentMenu：父节点
TextParent：父节点文本
ChildMenu：子节点容器
item：子节点
TextChild：子节点文本
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830164024475.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
然后将父节点改名字叫parentMenu，做成预制体：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830164104546.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
预制体放到Resources文件夹中：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830164115254.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
将子物体也制作成预制体：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830164217544.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
3、编写脚本ParentMenu.cs
这个脚本主要是作用是创建子物体：

```csharp
using System.Collections;
using UnityEngine;
using UnityEngine.UI;

public class ParentMenu : MonoBehaviour
{
    private GameObject childMenu;//子菜单的parent
    private RectTransform[] childs;//所有子菜单的rect
    private RectTransform itemRect;//子菜单的prefab
    private Vector3 offset;//单个子菜单的高度
    private int count;//子菜单的个数
    public bool isOpening { get; private set; }//父菜单是否展开
    public bool isCanClick { get; set; }//父菜单是否可以点击

    public void Init(RectTransform rect, int count)
    {
        //找到子节点
        childMenu = transform.Find("childMenu").gameObject;
        itemRect = rect;
        this.count = count;
        childs = new RectTransform[this.count];
        offset = new Vector3(0, itemRect.rect.height);
        for (int i = 0; i < this.count; i++)
        {
            childs[i] = Instantiate(itemRect, childMenu.transform);
        }
        childMenu.gameObject.SetActive(false);
        isOpening = false;
        isCanClick = true;
        GetComponent<Button>().onClick.AddListener(OnButtonClick);
    }

    void OnButtonClick()
    {
        if (!isCanClick) return;
        if (!isOpening)
            StartCoroutine(ShowChildMenu());
        else
            StartCoroutine(HideChildMenu());
    }

    IEnumerator ShowChildMenu()
    {
        childMenu.gameObject.SetActive(true);
        for (int i = 0; i < count; i++)
        {
            childs[i].localPosition -= i * offset;
            yield return new WaitForSeconds(0.1f);
        }
        isCanClick = true;
        isOpening = true;
    }

    IEnumerator HideChildMenu()
    {
        for (int i = count - 1; i >= 0; i--)
        {
            childs[i].localPosition += i * offset;
            yield return new WaitForSeconds(0.1f);
        }
        childMenu.gameObject.SetActive(false);
        isCanClick = true;
        isOpening = false;
    }
}

```

4、编写脚本FoldableMenu.cs
这个脚本主要是为了创建父物体，以及控制折叠菜单
```csharp
using System.Collections;
using UnityEngine;
using UnityEngine.UI;

public class FoldableMenu : MonoBehaviour
{
    private RectTransform content;//父物体的parent
    private TextAsset textAsset;//所有菜单信息
    private RectTransform parentRect;//父菜单的prefab
    private RectTransform[] parentArr;//所有父菜单的数组
    private RectTransform childRect;//子菜单的prefab
    private Vector3 parentOffset;//单个父菜单的高度
    private Vector3 childOffset;//单个父菜单的高度
    private int[] cntArr;//所有父菜单拥有的子菜单个数

    void Awake()
    {
        Init();
    }

    void Init()
    {
        //获取到父节点
        content = transform.Find("Viewport/Content").GetComponent<RectTransform>();
        //获取到menuinfo里面的信息  3 3 4 4 5 5
        textAsset = Resources.Load<TextAsset>("menuInfo");
        //获取到父物体 设置父物体的高度
        parentRect = Resources.Load<RectTransform>("parentMenu");
        parentOffset = new Vector3(0, parentRect.rect.height);
        //获取到子物体 设置子物体的高度
        childRect = Resources.Load<RectTransform>("item");
        childOffset = new Vector3(0, childRect.rect.height);
        //分割字符串 info = 3 3 4 4 5 5
        var info = textAsset.text.Split(',');//获取子菜单个数信息
        //数组的长度
        cntArr = new int[info.Length];
        //父菜单的数组
        parentArr = new RectTransform[info.Length];
        //初始化content高度 宽度不变 高度是所有父物体的总长
        content.sizeDelta = new Vector2(content.rect.width, parentArr.Length * parentRect.rect.height);
        //i = 6
        for (int i = 0; i < cntArr.Length; i++)
        {
            //创建服务器
            parentArr[i] = Instantiate(parentRect, content.transform);
            //坐标为 上一个父物体的宽度
            parentArr[i].localPosition -= i * parentOffset;
            //赋值
            cntArr[i] = int.Parse(info[i]);
            //父物体上面加载子物体 子物体数量为3 3 4 4 5 5
            parentArr[i].GetComponent<ParentMenu>().Init(childRect, cntArr[i]);
            int j = i;
            //父物体上面的button绑定事件
            parentArr[i].GetComponent<Button>().onClick.AddListener(() => { OnButtonClick(j); });
        }
    }

    void OnButtonClick(int i)
    {
        //如果iscanclick为false则return  因为已经点击过了 不能再点击了 除非升起来的时候将isCanClick改为true
        if (!parentArr[i].GetComponent<ParentMenu>().isCanClick)
            return;
        parentArr[i].GetComponent<ParentMenu>().isCanClick = false;
        //isopening 为true 执行 menuup 为flase执行menuDown
        if (!parentArr[i].GetComponent<ParentMenu>().isOpening)
            StartCoroutine(MenuDown(i));
        else
            StartCoroutine(MenuUp(i));
    }

    IEnumerator MenuDown(int index)
    {       
        for (int i = 0; i < cntArr[index]; i++)
        {
            //更新content高度
            content.sizeDelta = new Vector2(content.rect.width,
                content.rect.height + childOffset.y);
            for (int j = index + 1; j < parentArr.Length; j++)
            {
                parentArr[j].localPosition -= childOffset;
            }
            yield return new WaitForSeconds(0.1f);
        }     
    }

    IEnumerator MenuUp(int index)
    {
        for (int i = 0; i < cntArr[index]; i++)
        {
            //更新content高度
            content.sizeDelta = new Vector2(content.rect.width,
                content.rect.height - childOffset.y);
            for (int j = index + 1; j < parentArr.Length; j++)
            {
                parentArr[j].localPosition += childOffset;
            }
            yield return new WaitForSeconds(0.1f);
        }
    }
}

```
将这个脚本挂载到Scroll View上面：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019083016371671.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
运行，搞定！

</h1><h1 id="1.2">

### 第二种实现效果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830161942817.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
实现原理：这个也是用UGUI做的，不一样的是不需要容器组件，主要是寻找父节点，然后保存父节点的信息，下一个节点以父节点为目标进行偏移，或者以父节点为目标做子节点
优缺点：
优点是代码清晰，可扩展性强，不需要设计UGUI
缺点结构比较简单，没有实现多层级的功能
实现过程：
1、创建预制体
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830172049860.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
结构比较简单，两个Image，箭头的图片带Button组件（可以下拉和合并）
然后将预制体放到Resources文件夹中：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830173027574.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
2、编写PublicData.cs脚本
一个数据类
```csharp
using UnityEngine;

public class PublicData
{
    public static Transform parent;
    public static Vector2 currentPosition;
}

```
3、编写脚本Options_Triangle.cs
初始化子节点的函数

```csharp
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class Options_Triangle : MonoBehaviour
{
    public Button triangle;
    public Image content;
    public List<Transform> children = new List<Transform>();
    void Start()
    {
        triangle.onClick.AddListener(() =>
        {
            if (triangle.transform.eulerAngles.z == 270)
            {
                triangle.transform.eulerAngles = Vector3.zero;
                for (int i = 0; i < children.Count; i++)
                {
                    children[i].gameObject.SetActive(false);
                }
            }
            else
            {
                triangle.transform.eulerAngles = new Vector3(0, 0, -90);
                for (int i = 0; i < children.Count; i++)
                {
                    children[i].gameObject.SetActive(true);
                }
            }
        });

    }
}

```
4、编写ClickEvent.cs
这是一个层级菜单的编辑功能的脚本

```csharp
using UnityEngine;
using UnityEngine.UI;

public class ClickEvent : MonoBehaviour
{
    public Button add_options_t;
    public Button add_options;
    public Transform canvasParent;

    void Start()
    {
        add_options_t.onClick.AddListener(() =>
        {
            if (PublicData.parent == null)
            {
                PublicData.parent = Instantiate(Resources.Load<Transform>("Options_triangle"));
                PublicData.parent.transform.SetParent(canvasParent, false);
                PublicData.currentPosition = new Vector2(PublicData.parent.position.x, PublicData.parent.position.y);
            }
            else
            {
                Transform option = Instantiate(Resources.Load<Transform>("Options_triangle"));
                option.transform.SetParent(PublicData.parent, false);
                option.transform.position = new Vector3(PublicData.currentPosition.x + 30, PublicData.currentPosition.y - 32, 0);
                PublicData.parent.GetComponent<Options_Triangle>().triangle.transform.eulerAngles = new Vector3(0, 0, -90);
                PublicData.parent.GetComponent<Options_Triangle>().children.Add(option);
                PublicData.parent = option;
                PublicData.currentPosition = new Vector2(PublicData.parent.position.x, PublicData.parent.position.y);
            }

        });
        add_options.onClick.AddListener(() =>
        {
            if (PublicData.parent == null)
            {
                return;
            }
            PublicData.parent.GetComponent<Options_Triangle>().triangle.transform.eulerAngles = new Vector3(0, 0, -90);
            Transform content = Instantiate(Resources.Load<Transform>("Options"));
            content.transform.SetParent(PublicData.parent, false);
            content.transform.position = new Vector3(PublicData.currentPosition.x + 16, PublicData.currentPosition.y - 32, 0);
            PublicData.parent.GetComponent<Options_Triangle>().children.Add(content);
            PublicData.currentPosition = new Vector2(content.position.x - 16, content.position.y);

        });
    }
}

```

OK。将ClickEvent脚本挂载在场景中的任一物体身上就可以了

</h1><h1 id="1.3">

### 第三种实现效果
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019083016200749.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
实现原理：这个也是用UGUI做的，比较有特点的地方是没有使用一行代码，使用VerticalLayoutGroup和ContentSizeFitter组件的自动排序功能和Button的OnClick组件控制子物体的显示与隐藏来实现层级菜单的功能。
优缺点：
优点是不需要代码控制，简单易用
缺点是需要提前堆砌UI，工作量比较大，然后改动的时候耗费精力大
实现过程：
1、新建Scroll View
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830174049737.png)
2、父节点
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830174137950.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830174152662.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
父节点有一个Button和一个隐藏的Image

Button
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830174846928.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
有两个功能
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830174837617.png)
第一个就是显示跟自己位置坐标一样的子物体BtnSelecteStyle，因为BtnSelecteStyle是Button的子物体，所以BtnSelecteStyle就会挡住Button，为啥要挡住呢，因为还需要BtnSelecteStyle的OnClick将子节点收起来
BtnSelecteStyle的OnClick挂载的功能：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830175022633.png)
第二个就是显示子节点的容器也就是ImgBtnParentLayout

这个ImgBtnParentLayout就是子节点的容器
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830174912181.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
下面是几个子节点
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830174932987.png)

OK。可以了

</h1><h1 id="1.4">

### 第四种实现效果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830175602568.png)
实现原理：这个是用代码动态生成的，其中一个脚本主要用来创建父物体和子物体，以及父级关系，另一个脚本是设置位置，箭头变化，按钮功能的初始化
优缺点：
优点是代码清晰，结构明了，可以实现层级多级显示、多级目录的设置、树级菜单等
缺点是没有判断最后一个节点的代码，最后一个节点无法设置图片，最后一个节点的功能没有添加
实现过程：
1、首先也是制作预制体
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830180216879.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830180223949.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830180237930.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830180247235.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830180255528.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830180303672.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830180314647.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
ArrowButton和ArrowButton2都是为了控制子节点的关闭和显示，不同的是ArrowButton是左边的小按钮，还有一个图片显示的功能，ArrowButton2是整体的按钮，不显示，但是点击整体都可以实现显示和隐藏子节点的功能

资源：
![在这里述](https://img-blog.csdnimg.cn/20190830180540812.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019083018054784.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830180553931.png)
图片是白的，仔细看一下还是能看到的 - -，然后保存下载，放到项目中去


2、编写脚本ItemPanelBase.cs
这个脚本是设置节点的位置，以及箭头切换的脚本
```csharp
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public enum NameSort
{
    Default,
    Active,
    Passive
}
public class ItemPanelBase : MonoBehaviour
{
    private List<ItemPanelBase> childList;//子物体集合
    [HideInInspector]
    public Button downArrow;//下箭头按钮
    public Button downArrow2;//下箭头按钮
    public Sprite down, right, dot;
    public bool isOpen { get; set; }//子物体开启状态
    private Vector2 startSize;//起始大小

    NameSort m_NameSore = NameSort.Default;
    string m_Name;

    private void Awake()
    {
        childList = new List<ItemPanelBase>();
        downArrow = this.transform.Find("ContentPanel/ArrowButton").GetComponent<Button>();
        downArrow.onClick.AddListener(() =>
        {
            if (isOpen)
            {
                CloseChild();
                isOpen = false;
            }
            else
            {
                OpenChild();
                isOpen = true;
            }
        });
        downArrow2 = this.transform.Find("ContentPanel/ArrowButton2").GetComponent<Button>();
        downArrow2.onClick.AddListener(() =>
        {
            if (isOpen)
            {
                CloseChild();
                isOpen = false;
            }
            else
            {
                OpenChild();
                isOpen = true;
            }
        });
        startSize = this.GetComponent<RectTransform>().sizeDelta;
        isOpen = false;
    }

    //添加子物体到集合
    private void AddChild(ItemPanelBase parentItemPanelBase)
    {
        childList.Add(parentItemPanelBase);
        if (childList.Count >= 1)
        {
            downArrow.GetComponent<Image>().sprite = right;
        }
    }

    //设置当前对象的名字值
    public void SetName(string _name,NameSort m_sort)
    {
        m_Name = _name;
        m_NameSore = m_sort;
    }

    /// <summary>
    /// 设置父物体，父物体不为一级菜单
    /// </summary>
    /// <param name="parentItemPanelBase"></param>
    public void SetItemParent(ItemPanelBase parentItemPanelBase)
    {
        this.transform.parent = parentItemPanelBase.transform;
        parentItemPanelBase.AddChild(this);
        this.GetComponent<VerticalLayoutGroup>().padding = new RectOffset((int)parentItemPanelBase.downArrow.GetComponent<RectTransform>().sizeDelta.x, 0, 0, 0);
        if (parentItemPanelBase.isOpen)
        {

            this.GetComponent<ItemPanelBase>().AddParentSize((int)this.gameObject.GetComponent<RectTransform>().sizeDelta.y);
        }
        else
        {
            this.transform.gameObject.SetActive(false);
        }
    }

    /// <summary>
    /// 设置父物体，父物体为一级菜单
    /// </summary>
    /// <param name="tran"></param>
    public void SetBaseParent(Transform tran)
    {
        this.transform.parent = tran;
    }

    /// <summary>
    /// 填充Item数据
    /// </summary>
    /// <param name="_name">名字</param>
    public void InitPanelContent(string _name)
    {
        transform.Find("ContentPanel/Text").GetComponent<Text>().text = _name;
    }

    /// <summary>
    /// 增加一个子物体后更新Panel大小
    /// </summary>
    /// <param name="change"></param>
    public void UpdateRectTranSize(int change)
    {
        this.gameObject.GetComponent<RectTransform>().sizeDelta = new Vector2(startSize.x, this.gameObject.GetComponent<RectTransform>().sizeDelta.y + change);
    }
    /// <summary>
    /// 增加父物体高度
    /// </summary>
    /// <param name="parentItem"></param>
    /// <param name="change"></param>
    public void AddParentSize(int change)
    {
        if (this.transform.parent.GetComponent<ItemPanelBase>() != null)
        {
            this.transform.parent.GetComponent<ItemPanelBase>().UpdateRectTranSize(change);
            this.transform.parent.GetComponent<ItemPanelBase>().AddParentSize(change);
        }
    }

    /// <summary>
    /// 关闭子物体列表
    /// </summary>
    public void CloseChild()
    {
        if (childList.Count == 0) return;
        foreach (ItemPanelBase child in childList)
        {
            child.gameObject.SetActive(false);
            child.GetComponent<ItemPanelBase>().AddParentSize(-(int)child.gameObject.GetComponent<RectTransform>().sizeDelta.y);
        }
        downArrow.GetComponent<Image>().sprite = right;
    }

    /// <summary>
    /// 打开子物体列表
    /// </summary>
    public void OpenChild()
    {
        if (childList.Count == 0)
        {
            if (m_Name == "125K读卡器" && m_NameSore == NameSort.Active)
            {
                Debug.Log("125k设备");
            }
            return;
        }

        foreach (ItemPanelBase child in childList)
        {
            child.gameObject.SetActive(true);
            child.GetComponent<ItemPanelBase>().AddParentSize((int)child.gameObject.GetComponent<RectTransform>().sizeDelta.y);
        }
        downArrow.GetComponent<Image>().sprite = down;

    }
}

```
3、编写脚本PullDownList.cs
这个就是创建层级菜单的脚本：
比较繁琐

```csharp
using System.Collections.Generic;
using UnityEngine;

public class PullDownList : MonoBehaviour
{
    private List<GameObject> itemPanelList;
    public GameObject itemPanel;

    private void Awake()
    {
        itemPanelList = new List<GameObject>();
    }

    void Start()
    {
        for (int i = 0; i < 27; i++)
        {
            //一级菜单
            if(i == 0)
            {
                GameObject newItemPanel = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel);
                newItemPanel.GetComponent<ItemPanelBase>().SetBaseParent(this.transform);
                newItemPanel.GetComponent<ItemPanelBase>().InitPanelContent("层级一");
            }
            //二级菜单
            if (i == 1)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[0].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级二");
            }
            else if (i == 2)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[0].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级二");
            }
            //三级菜单
            else if (i == 3)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[1].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级三");
            }
            else if (i == 4)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[1].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级三");
            }
            else if (i == 5)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[1].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级三");
            }
            else if (i == 6)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[1].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级三");
            }
            else if (i == 7)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[2].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级三");
            }
            else if (i == 8)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[2].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级三");
            }
            else if (i == 9)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[2].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级三");
            }
            else if (i == 10)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[2].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级三");
            }
            //四级菜单
            else if (i == 11)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[3].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级四");
                newItemPanel2.GetComponent<ItemPanelBase>().SetName("层级四", NameSort.Active);
            }
            else if (i == 12)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[3].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级四");
                newItemPanel2.GetComponent<ItemPanelBase>().SetName("层级四", NameSort.Active);
            }
            else if (i == 13)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[4].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级四");
                newItemPanel2.GetComponent<ItemPanelBase>().SetName("层级四", NameSort.Active);
            }
            else if (i == 14)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[4].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级四");
                newItemPanel2.GetComponent<ItemPanelBase>().SetName("层级四", NameSort.Active);
            }
            else if (i == 15)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[5].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级四");
                newItemPanel2.GetComponent<ItemPanelBase>().SetName("层级四", NameSort.Active);
            }
            else if (i == 16)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[5].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级四");
                newItemPanel2.GetComponent<ItemPanelBase>().SetName("层级四", NameSort.Active);
            }
            else if (i == 17)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[6].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级四");
                newItemPanel2.GetComponent<ItemPanelBase>().SetName("层级四", NameSort.Active);
            }
            else if (i == 18)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[6].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级四");
                newItemPanel2.GetComponent<ItemPanelBase>().SetName("层级四", NameSort.Active);
            }
            else if (i == 19)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[7].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级四");
                newItemPanel2.GetComponent<ItemPanelBase>().SetName("层级四", NameSort.Passive);
            }
            else if (i == 20)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[7].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级四");
                newItemPanel2.GetComponent<ItemPanelBase>().SetName("层级四", NameSort.Passive);
            }
            else if (i == 21)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[8].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级四");
                newItemPanel2.GetComponent<ItemPanelBase>().SetName("层级四", NameSort.Passive);
            }
            else if (i == 22)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[8].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级四");
                newItemPanel2.GetComponent<ItemPanelBase>().SetName("层级四", NameSort.Passive);
            }
            else if (i == 23)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[9].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级四");
                newItemPanel2.GetComponent<ItemPanelBase>().SetName("层级四", NameSort.Passive);
            }
            else if (i == 24)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[9].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级四");
                newItemPanel2.GetComponent<ItemPanelBase>().SetName("层级四", NameSort.Passive);
            }
            else if (i == 25)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[10].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级四");
                newItemPanel2.GetComponent<ItemPanelBase>().SetName("层级四", NameSort.Passive);
            }
            else if (i == 26)
            {
                GameObject newItemPanel2 = Instantiate(itemPanel);
                itemPanelList.Add(newItemPanel2);
                newItemPanel2.GetComponent<ItemPanelBase>().SetItemParent(itemPanelList[10].GetComponent<ItemPanelBase>());
                newItemPanel2.GetComponent<ItemPanelBase>().InitPanelContent("层级四");
                newItemPanel2.GetComponent<ItemPanelBase>().SetName("层级四", NameSort.Passive);
            }
        }
    }
}

```
将脚本挂载在Panel上面
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830181011446.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

OK，大功告成

</h1><h1 id="1.5">

### 第五种实现效果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830162435766.png)
实现原理：这个是用UI硬堆砌起来的层级菜单，然后通过代码控制对象的隐藏和显示，即可实现层级菜单的折叠与下拉功能，主要用到GridLayoutGroup组件来排序与更新
优缺点：
优点是操作简单，代码也简单，不需要太多的理解，然后可以显示多级菜单，多级内容，以及最后一个节点的功能与图片的设置功能
缺点是需要提前堆砌UI，可扩容性差，前期工作量大，然后后期修改工作量大，最重要的是我觉得这种实现方式蛮low的
实现过程：
1、显示制作UI
Panel上面挂载GridLayoutGroup组件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830182632893.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
制作UI
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019083018175336.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830182555992.png)
UI的话就很简单，一个Button下面两个子物体一个text一个Image，text是显示内容，image是显示箭头


这时候就有人问了，那子物体怎么办，子物体也是同样的结构![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830182826652.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
就是把image往后拉了一下

三级菜单也一样：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830182905651.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
再加一个一级菜单：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830182929376.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

是不是so easy....哈哈哈 真的好low

脚本功能就很简单
一级菜单控制它往下的所有子节点的隐藏于显示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830183104880.png)
二级菜单控制它往下的所有子节点的隐藏于显示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830183126751.png)

以此类推。。。。

2、编辑代码PullDown.cs

```csharp
using UnityEngine;
using UnityEngine.UI;

public class PullDown : MonoBehaviour
{
    public Button m_Btn1;//一级菜单按钮
    public Button m_Btn2;//二级菜单按钮
    public Button m_Btn3;//三级菜单按钮

    bool m_is1;//全局参数 控制开关
    bool m_is2;//全局参数 控制开关

    void Start()
    {
        m_is1 = true;
        m_is2 = true;
        m_Btn1.onClick.AddListener(Btn1_Event);
        m_Btn2.onClick.AddListener(Btn2_Event);
    }

    public void Btn1_Event()
    {
        if (m_is1)
        {
            m_Btn2.gameObject.SetActive(false);
            m_Btn3.gameObject.SetActive(false);
            m_is1 = false;
        }
        else
        {
            m_Btn2.gameObject.SetActive(true);
            m_Btn3.gameObject.SetActive(true);
            m_is1 = true;
        }
    }

    public void Btn2_Event()
    {
        if (m_is2)
        {
            m_Btn3.gameObject.SetActive(false);
            m_is2 = false;
        }
        else
        {
            m_Btn3.gameObject.SetActive(true);
            m_is1 = true;
        }
    }
}
```



OK了，快去试试吧
</h1>
