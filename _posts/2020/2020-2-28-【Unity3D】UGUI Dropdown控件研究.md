---
layout: post
category: Unity3D-Daily
title: 【Unity3D】UGUI Dropdown控件研究
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
Dropdown下拉列表，控件还是很强大的，做UI的时候用的比较多，现在就将Dropdown使用中的一些经验总结起来，分享给大家了 


## 二、参考资料
[UGUI 中Dropdown控件的使用经验](https://blog.csdn.net/liu_guozhu/article/details/52813444)
[Unity3D UGUI中的dropdown控件使用总结](https://blog.csdn.net/liu442620190/article/details/52702204)
[Unity(一)关于Unity Dropdown控件的使用心得](https://blog.csdn.net/linzhonglong/article/details/71195411)

## 三、正文
对于Dropdown控件的研究，我将分成这么几个部分：

- 1、控件的组成以及属性面板介绍
- 2、控件的初始化以及内容显示
- 3、增加节点以及删除节点
- 4、事件监听方式

### 1、控件的组成以及属性面板介绍
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823172706796.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
Label是显示初始化的文字
Arrow是显示初始化的下拉箭头
Template是Dropdown的模板样式
Item Background是每一个Item的背景图片
Item Checkmark是每一个Item的下拉框图片
Item Label是每一个Item的文字显示内容
Scrollbar是一个下拉框
其中Item Background和Item Checkmark的图集资源我们可以提前更改。

然后我们看一下Dropdown的属性面板：
![在这里插入图片描述](https://img-blog.csdn.net/20170505142708651?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvTGluWmhvbmdsb25n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
Caption Text和Caption Image是作为下拉列表首选项的文字和图片显示，也是我们每次选择后的内容，因此可代码调用获取

Item Text作为下拉列表中每个item的文字显示，Item Image可以用来扩展模板增加内容

Value值会随着下拉列表选项的不同而变化，dropdown.value

Options选项栏内：可以动态赋值给Item对象  Dropdown.OptionData

### 2、控件的初始化以及内容显示
**初始化文字内容**
```csharp
using UnityEngine;
using UnityEngine.UI;

public class TestDropdown : MonoBehaviour
{
    public Dropdown Drd_IPList;

    private void Start()
    {
        InitDropdown();
    }

    private void InitDropdown()
    {
        //清空默认节点
        Drd_IPList.options.Clear();

        //初始化
        Dropdown.OptionData op1 = new Dropdown.OptionData();
        op1.text = "220.110.1.10";
        Drd_IPList.options.Add(op1);

        Dropdown.OptionData op2 = new Dropdown.OptionData();
        op2.text = "220.110.1.11";
        Drd_IPList.options.Add(op2);

        Dropdown.OptionData op3 = new Dropdown.OptionData();
        op3.text = "220.110.1.12";
        Drd_IPList.options.Add(op3);
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823173640421.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823173613993.png)
**同时初始化文字和图片**

```csharp
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class TestDropdown : MonoBehaviour
{
    public Dropdown Drd_IPList;
    public List<Sprite> m_Sprite;

    List<string> m_TextContent;
    Dropdown.OptionData m_TempData;

    private void Start()
    {
        AddTextContent();
        InitDropdown();
    }

    private void AddTextContent()
    {
        for (int i = 0; i < 3; i++)
        {
            m_TextContent.Add("220.110.1.1" + i);
        }
    }

    private void InitDropdown()
    {
        //清空默认节点
        Drd_IPList.options.Clear();

        //初始化
        for (int i = 0; i < 3; i++)
        {
            m_TempData = new Dropdown.OptionData();
            m_TempData.text = m_TextContent[i];
            m_TempData.image = m_Sprite[i];
            Drd_IPList.options.Add(m_TempData);
        }
        //初始选项的显示
        Drd_IPList.captionText.text = m_TextContent[0];
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823174554713.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823174754398.png)

### 3、增加节点以及删除节点
**添加节点**
```csharp
    //添加节点
    public void AddItem()
    {
        m_TempData = new Dropdown.OptionData();
        m_TempData.text = "新添加的节点";
        Drd_IPList.options.Add(m_TempData);
    }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823175935313.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823175943763.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823180001483.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823180042938.png)
**删除节点**

```csharp
	//删除节点
    public void DelectItem()
    {
        //删除第一个节点
        m_TempData = Drd_IPList.options[0];
        Drd_IPList.options.Remove(m_TempData);
    }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823180453217.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019082318051056.png)
删除后：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823180521911.png)
### 4、事件监听方式
**使用Dropdown自带的监听事件**

```csharp
using UnityEngine;
using UnityEngine.UI;
[RequireComponent(typeof(Dropdown))]
public class Drop : MonoBehaviour
{
    private Dropdown drop;
    void Start()
    {
        drop = this.GetComponent<Dropdown>();
        drop.onValueChanged.AddListener(Change);
    }
    private void Change(int index)
    {
        Debug.Log(index);       
        switch (index)
        {
            case 0: break;
            case 1: break;
            default: break;
        }
    }
```

**使用ISelectHandler接口进行事件监听**

```csharp
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.EventSystems;
public class Drop : MonoBehaviour,ISelectHandler
{
    public Dropdown drop;
    private int lastIndex;
   
    public void OnSelect(BaseEventData eventData)
    {
        //避免点击下拉列表item和dropdown重复调用
        if (drop.value == lastIndex) return;
        
        //处理逻辑
        //
 
        Debug.Log("OnSelect=" + drop.value);
        lastIndex = drop.value;
    }
```

**在Dropdown面板中使用脚本监听**

```csharp
using UnityEngine;
using UnityEngine.UI;

public class TestDropdown : MonoBehaviour
{
    public Dropdown Drd_IPList;

    //事件监听
    public void EventListening()
    {
        switch (Drd_IPList.value)
        {
            case 0:
                Debug.Log(0);
                break;
            case 1:
                Debug.Log(1);
                break;
            case 2:
                Debug.Log(2);
                break;
            default:
                break;
        }
    }
}

```
将脚本挂在Main Camera上面（当然，任何物体都可以），然后将Dropdown拖入卡槽中：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823181236694.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
将Dropdown下面的On Value Changed增加方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823181246980.png)
运行起来，可以看到控制台打印的信息
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823181255318.png)
