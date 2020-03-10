---
layout: post
category: Unity3D-Daily
title: 【Unity3D】UGUI对image控件检测鼠标按下和抬起
tagline: by 恬静的小魔龙
tag: Unity3D
---

在UGUI中对image控件检测鼠标按下和抬起使用OnPointerDown和OnPointerUp方法

其中OnPointerDown方法需要类继承IPointerDownHandler接口，而OnPointerUp方法需要类继承IPointerUpHandler接口。

<h1>OnPointerDown方法</h2>

```
public void OnPointerDown(PointerEventData eventData)
{

}
```
<h1>OnPointerUp方法</h1>

```
public void OnPointerUp(PointerEventData eventData) 
{

}
```
当鼠标在控件上按下时，会触发OnPointerDown方法；当鼠标抬起时会会触发OnPointerUp方法。 
- 当需要使用这两个方法时，需要调用using UnityEngine.EventSystems;

希望完成的效果 
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMTI1MTEzNDQ0ODY3?x-oss-process=image/format,png)

鼠标按住“开始”控件并下滑时，检测该控件的位置，当移动的距离超过一半时如果松开，控件会自动移动到背景控件下方，若为超过距离的一半，松开鼠标，控件会返回上端。 
1. 为背景控件添加ScrollRect方法，并将content属性设置为开始控件。 
2. 设置ScrollRect方法，勾选Vertical，使开始控件只能上下移动。 
3. 通过代码控制开始控件的位置，判断不同位置控件的变化。 
源代码如下

```
using UnityEngine;
using System.Collections;
using UnityEngine.UI;
using UnityEngine.EventSystems;

public class StartButton : MonoBehaviour,IPointerDownHandler,IPointerUpHandler{

    private Transform ButtonStart;
    private Image ButtonImage;

    private bool isPointerDown;
    private Vector3 InitMousePos;


    void Awake() {
        ButtonStart = this.transform.Find("Start").transform;
        ButtonImage=ButtonStart.GetComponent<Image>();
    }
    void Start() {
        ButtonStart.localPosition = new Vector3(ButtonStart.localPosition.x, 60f, ButtonStart.transform.localPosition.z);
        InitMousePos = Vector3.zero;
    }
    void Update() {
        UpdateButton();
    }

    //根据Y值来改变游戏状态
    private void UpdateButton()
    {
        if (isPointerDown)
        {
            if (ButtonStart.localPosition.y > 60f || ButtonStart.localPosition.y < -60f)
            {
                float newY = (Mathf.Abs(ButtonStart.localPosition.y) / ButtonStart.localPosition.y) * 60f;
                if (newY <= 0) {
                    ButtonImage.color = new Color(104, 255, 0, 255);
                }
                ButtonStart.localPosition = new Vector3(ButtonStart.localPosition.x, newY, ButtonStart.transform.localPosition.z);
            }
        }
        else {
            float y = ButtonStart.localPosition.y;
            if (y <= 0)
            {
                ButtonStart.localPosition = new Vector3(ButtonStart.localPosition.x, -60f, ButtonStart.transform.localPosition.z);
                ButtonImage.color = new Color(104, 255, 0, 255);
            }
            else
            {
                ButtonStart.localPosition = new Vector3(ButtonStart.localPosition.x, 60f, ButtonStart.transform.localPosition.z);
            }
            if (ButtonStart.localPosition.y == -60f) {
                this.GetComponent<ScrollRect>().enabled = false;
                StartCoroutine(WaitAndSkip());
            }

        }
    }

    //控制场景等待、跳转
    private IEnumerator WaitAndSkip() {
        yield return new WaitForSeconds(0.5f);
        Application.LoadLevel(1);
    }


    //检测鼠标按下与抬起
    public void OnPointerDown(PointerEventData eventData)
    {
        isPointerDown = true;
    }
    public void OnPointerUp(PointerEventData eventData) {
        isPointerDown = false;
    }
}
```
