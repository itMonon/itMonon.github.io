---
layout: post
category: Unity3D-Daily
title: 	【Unity3D】UGUI Button绑定事件的几种方法
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
今天分享一下UGUI Button绑定事件的几种方法，以及优点和缺点
有哪些地方不懂的小伙伴也可以联系我的QQ，我的QQ就在博客链接中隐藏着，看能不能找到咯


## 二、正文
我们先写一个Button的响应脚本ButtonTest.cs

```csharp
using UnityEngine;
using UnityEngine.UI;

public class ButtonTest : MonoBehaviour
{
    public Text m_Text;
    public void ButtonOnClickEvent()
    {
        m_Text.text = "鼠标点击";
    }
}
```
### 一、可视化创建及事件绑定
点击Button组件上的OnClick的+号
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190430090916457.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
然后把绑定脚本的对象，赋值到这个Button组件上
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190430091558283.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190430092048350.gif)
### 二、通过直接绑定脚本来绑定事件
使用Button组件自带的onClick.AddListener方法
代码

```csharp
using UnityEngine;
using UnityEngine.UI;

public class ButtonTest : MonoBehaviour
{
    public Button m_Button;
    public Text m_Text;
    void Start()
    {
        m_Button.onClick.AddListener(ButtonOnClickEvent);
    }
    public void ButtonOnClickEvent()
    {
        m_Text.text = "鼠标点击";
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190430092048350.gif)
### 三、通过射线监听点击到的物体来绑定事件
代码

```csharp
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.EventSystems;
using UnityEngine.UI;

public class ButtonTest : MonoBehaviour
{
    public Text m_Text;

    void Update()
    {
        if (Input.GetMouseButtonDown(0))
        {
            if (OnePointColliderObject() != null)
            {
                if (OnePointColliderObject().name == "Button" || OnePointColliderObject().name == "Text")
                {
                    ButtonOnClickEvent();
                }
            }
        }
    }

    //点击对象获取到对象的名字
    public GameObject OnePointColliderObject()
    {
        //存有鼠标或者触摸数据的对象
        PointerEventData eventDataCurrentPosition = new PointerEventData(EventSystem.current);
        //当前指针位置
        eventDataCurrentPosition.position = new Vector2(Input.mousePosition.x, Input.mousePosition.y);
        //射线命中之后的反馈数据
        List<RaycastResult> results = new List<RaycastResult>();
        //投射一条光线并返回所有碰撞
        EventSystem.current.RaycastAll(eventDataCurrentPosition, results);
        //返回点击到的物体
        if (results.Count > 0)
            return results[0].gameObject;
        else
            return null;
    }

    public void ButtonOnClickEvent()
    {
        m_Text.text = "鼠标点击";
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190430092048350.gif)
### 四、通过 EventTrigger 实现按钮点击事件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190430094652510.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
编写代码

```csharp
using UnityEngine;
using UnityEngine.EventSystems;
using UnityEngine.UI;

[RequireComponent(typeof(EventTrigger))]
public class ButtonTest : MonoBehaviour
{
    public Text m_Text;

    void Start()
    {
        Button btn = transform.GetComponent<Button>();
        EventTrigger trigger = btn.gameObject.GetComponent<EventTrigger>();
        EventTrigger.Entry entry = new EventTrigger.Entry
        {
            // 鼠标点击事件
            eventID = EventTriggerType.PointerClick,
            // 鼠标进入事件 entry.eventID = EventTriggerType.PointerEnter;
            // 鼠标滑出事件 entry.eventID = EventTriggerType.PointerExit;
            callback = new EventTrigger.TriggerEvent()
        };
        entry.callback.AddListener(ButtonOnClickEvent);
        // entry.callback.AddListener (OnMouseEnter);
        trigger.triggers.Add(entry);
    }

    public void ButtonOnClickEvent(BaseEventData pointData)
    {
        m_Text.text = "鼠标点击";
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190430092048350.gif)
### 五、通过通用类 UIEventListener 来处理Button响应事件
UIEventListener.cs

```csharp
using UnityEngine;
using UnityEngine.EventSystems;

public class UIEventListener : MonoBehaviour, IPointerClickHandler
{
    // 定义事件代理
    public delegate void UIEventProxy();
    // 鼠标点击事件
    public event UIEventProxy OnClick;

    public void OnPointerClick(PointerEventData eventData)
    {
        if (OnClick != null)
            OnClick();
    }
}
```
ButtonTest.cs

```csharp
using UnityEngine;
using UnityEngine.EventSystems;
using UnityEngine.UI;

[RequireComponent(typeof(EventTrigger))]
public class ButtonTest : MonoBehaviour
{
    public Text m_Text;

    void Start()
    {
        Button btn = this.GetComponent<Button>();
        UIEventListener btnListener = btn.gameObject.AddComponent<UIEventListener>();

        btnListener.OnClick += delegate () {
            ButtonOnClickEvent();
        };
    }

    public void ButtonOnClickEvent()
    {
        m_Text.text = "鼠标点击";
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190430092048350.gif)

