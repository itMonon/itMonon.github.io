---
layout: post
category: Unity3D-Daily
title: 【Unity3D】Unity3D 鼠标移动到UGUI上面显示文字
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
有时候图标不能很好的说明这个功能的解释，就需要一些说明性文字显示。就比如可以在鼠标移动到UI上面的时候显示文字。
那么如何在UGUI上，鼠标移动上去显示文字说明呢。
大家都知道，当鼠标移动到button按钮上面的时候会出现变化，主要是button这个组件在控制
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190128101945806.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
既然可以控制颜色，就一定有状态捕捉的枚举
然后就找到了这个
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190128102218792.png)
接下来就是重写Button类了


## 二、实现效果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190128102555568.gif)


## 三、实现步骤
1.新建一个button，然后remove掉原来的button组件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190128143148824.png)
2.新建TestButton.cs脚本，编写脚本

```csharp
using UnityEngine;
using UnityEngine.UI;

public class TestButton : Button
{
    enum Selection
    {
        Normal,
        Highlighted,
        Pressed,
        Disabled
    }
    Selection selection;

    protected override void DoStateTransition(SelectionState state, bool instant)
    {
        base.DoStateTransition(state, instant);
        switch (state)
        {
        	//四种状态
            case SelectionState.Normal:
                selection = Selection.Normal;
                break;
            case SelectionState.Highlighted:
                selection = Selection.Highlighted;
                break;
            case SelectionState.Pressed:
                selection = Selection.Pressed;
                break;
            case SelectionState.Disabled:
                selection = Selection.Disabled;
                break;
            default:
                break;
        }
    }

    private void OnGUI()
    {
        GUI.skin.box.fontSize = 10;
        switch (selection)
        {
            case Selection.Highlighted:
                GUI.Box(new Rect(Input.mousePosition.x, Screen.height - Input.mousePosition.y, 100, 25), "Highlighted");
                break;
            case Selection.Pressed:
                GUI.Box(new Rect(Input.mousePosition.x, Screen.height - Input.mousePosition.y, 100, 25), "Pressed");
                break;
            default:
                break;
        }
    }
}
```
3.挂载到button按钮上
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190128143242955.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

OK了 。


## 四、使用EventTriggerListener组件
可以直接使用EventTriggerListener 组件 不过会覆盖其它事件
也可以单独使用接口，不会对其它事件造成影响
IPointerEnterHandler 当鼠标进入对象时
IPointerExitHandler 当鼠标退出对象时
IPointerDownHandler 当鼠标点下对象时
IPointerUpHandler 当鼠标抬起时
IPointerClickHandler 当鼠标点击时
IBeginDragHandler 鼠标开始拖动时
IDragHandler 鼠标拖动时
IEndDragHandler 拖动结束时
IScrollHandler 鼠标滚轮时

这些等以后再详细介绍

