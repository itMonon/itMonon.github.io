---
layout: post
category: Unity3D-Plugin
title: 【EasyTouch】Unity3d 插件研究之EasyTouch插件
tagline: by 恬静的小魔龙
tag: Unity3D
---

<h1>原文链接</h1>
https://blog.csdn.net/q764424567/article/details/78426905
这篇文章参考了很多博客，然后加入了一些自己的理解，从了解到深入都有比较详细的例子参考，有不对的地方希望大家可以指出来。thank！

<h1>参考资料：</h1>
Unity3D教程之EasyTouch插件
http://www.newbieol.com/information/564.html
Unity3D EasyTouch 初步使用教程（详细）
http://blog.csdn.net/lifeonelive/article/details/47974905
Unity插件EasyTouch学习记录
http://blog.csdn.net/crazyape/article/details/71597452
【Unity插件】EasyTouch5教程
http://blog.csdn.net/weixin_38158625/article/details/72673294
Unity插件——EasyTouch的使用
http://blog.csdn.net/u014086857/article/details/52087379


<h1>首先是下载链接：</h1>
第一：http://www.newbieol.com
第二：https://www.assetstore.unity3d.com/cn/#!/content/3322
第三：http://download.csdn.net/detail/s10141303/6962919
第四：http://pan.baidu.com/s/1o6Bt2bS

<h1>创建步骤：</h1>
点击菜单栏的Tools->Hedgehog Team->Easy Touch->Extensions->Add a new Joystick 完成以上骤，此时就会在Game试图左下角看到创建了虚拟遥感的实例。
　　![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMTAyMTY0NjI2NDY4?x-oss-process=image/format,png)
　　![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMTAyMTY0NjM0ODI2?x-oss-process=image/format,png)
　　
　　<h1>属性面板的组件预览</h1>
　　![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMTAyMTY0ODA1NDkx?x-oss-process=image/format,png)
　　<h2>属性面板组件参数</h2>
　　<h3>joystick properties</h3>
　　![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMTAyMTY1MjMzNDk0?x-oss-process=image/format,png)
　　Joystick properties下的Joystick name的命名很重要（名字要设置好，脚本代码可以根据这个名字找到是哪个摇杆触发的），等下脚本要用到它。
　　<h3>joystick position & size</h3>
　　![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMTAyMTY1MjQ2MzQ1?x-oss-process=image/format,png)
　　<h3>joystick axes properties & events</h3>
　　![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMTAyMTY1MjU0MzY3?x-oss-process=image/format,png)
　　Interaction type(事件驱动类型）选择Event Notification。
　　<h3>joystick textures</h3>
　　![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMTAyMTY1MzAzMDc4?x-oss-process=image/format,png)
　　参数Speed，控制的是摇杆的x，y轴向的灵敏度，都改为1即可。

以上设置完成后，我们新建一个脚本Move.cs

添加如下代码。

```
using UnityEngine;
using System.Collections;

public class Move : MonoBehaviour {

        void OnEnable()  

        {    
               EasyJoystick.On_JoystickMove += OnJoystickMove;  
        }  
                       //  此函数是摇杆移动中所要处理的事
        void OnJoystickMove(MovingJoystick move)  
          {    
              if (move.joystickName != "new joystick")       //  在这里的名字new joystick 就是上面所说的很重要的名字，在上面图片中joystickName的你修改了什么名字，这里就要写你修改的好的名字（不然脚本不起作用）。
               {  
                         return;  
               }  

       float PositionX = move.joystickAxis.x;       //   获取摇杆偏移摇杆中心的x坐标
       float PositionY = move.joystickAxis.y;      //    获取摇杆偏移摇杆中心的y坐标

              if (PositionY != 0 || PositionX != 0)  
               {                //  设置控制角色或物体方块的朝向（当前坐标+摇杆偏移量）
                       transform.LookAt(new Vector3(transform.position.x + PositionX, transform.position.y, transform.position.z + PositionY));  
                                  //  移动角色或物体的位置（按其所朝向的位置移动）
                       transform.Translate(Vector3.forward * Time.deltaTime * 25);  
               }  
         } 
}  
```

　　
把此脚本放到需要用摇杆控制的物体上，即可实现摇杆控制物体移动。


<h1>实例一</h1>
场景里放一个cube，当我用一根手指在屏幕上滑动的时候，我需要cube旋转；当我对它拖拽时，需要它跟着手指移动；当我用两根手指滑动时相机平面移动；当我双指向内捏或者向外拉时相机拉近或远。

![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMTAzMDg0NDA1MTQx?x-oss-process=image/format,png)
1.首先添加 easytouch游戏对象，它本身有许多设置选项，我大概过了一下，然后，啥也没记住。
这个简易demo里也不需要修改什么。
2.创建cube
3.创建脚本，并挂给cube
4.编写脚本

![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMTAzMDg0NDM0NDA2?x-oss-process=image/format,png)

这是easytouch中订阅事件的方法，EasyTouch下定义了各种类型的事件，我们只需要编写相应处理方法，然后+=订阅。在OnEnable中订阅，在OnDisable和OnDestroy中取消订阅。方法要传一个Gesture类型的参数，包含了手势的数据。

我的EasyTouchTest

```
using System.Collections;  
using System.Collections.Generic;  
using UnityEngine;  
using HedgehogTeam.EasyTouch;  
using UnityEngine.UI;  
public class EasyTouchTest : MonoBehaviour {  
    public Button BtnReset;  
    // Use this for initialization  
    void Start () {  
          
    }  
      
    // Update is called once per frame  
    void Update () {  
          
    }  
  
    void OnEnable() {  
        EasyTouch.On_Swipe += On_Swipe;  
        EasyTouch.On_Drag += On_Drag;  
        EasyTouch.On_Swipe2Fingers += On_Swipe2Fingers;  
        EasyTouch.On_Pinch += On_Pinch;  
        BtnReset.onClick.AddListener(ResetScene);  
    }  
  
    void OnDisable() {  
        EasyTouch.On_Swipe -= On_Swipe;  
        EasyTouch.On_Drag -= On_Drag;  
        EasyTouch.On_Swipe2Fingers -= On_Swipe2Fingers;  
        EasyTouch.On_Pinch -= On_Pinch;  
        BtnReset.onClick.RemoveListener(ResetScene);  
    }  
  
    void OnDestroy() {  
        EasyTouch.On_Swipe -= On_Swipe;  
        EasyTouch.On_Drag -= On_Drag;  
        EasyTouch.On_Swipe2Fingers -= On_Swipe2Fingers;  
        EasyTouch.On_Pinch -= On_Pinch;  
        BtnReset.onClick.RemoveListener(ResetScene);  
    }  
  
    /// <summary>  
    /// 重置cube和相机  
    /// </summary>  
    void ResetScene() {  
        transform.position = Vector3.zero;  
        transform.rotation = Quaternion.Euler(Vector3.zero);  
        Camera.main.transform.position = new Vector3(0, 0, -10);  
    }  
  
    /// <summary>  
    /// 滑动使cube旋转  
    /// </summary>  
    /// <param name="ges"></param>  
    void On_Swipe(Gesture ges) {  
        Vector3 vec = new Vector3(ges.deltaPosition.y, ges.deltaPosition.x, 0);  
        transform.Rotate(vec ,Space.World);  
    }  
  
    /// <summary>  
    /// 拖拽移动cube  
    /// </summary>  
    /// <param name="ges"></param>  
    void On_Drag(Gesture ges) {  
        if (ges.pickedObject == gameObject) {  
            transform.position = ges.GetTouchToWorldPoint(10);//相机z=-10 cube 0  
        }  
    }  
  
    /// <summary>  
    /// 双指滑动 平面移动相机  
    /// </summary>  
    /// <param name="ges"></param>  
    void On_Swipe2Fingers(Gesture ges) {  
        Camera.main.transform.Translate(new Vector3(-ges.deltaPosition.x, -ges.deltaPosition.y, 0));  
    }  
  
    /// <summary>  
    /// 拉近拉远相机  
    /// </summary>  
    /// <param name="ges"></param>  
    void On_Pinch(Gesture ges) {  
        Camera.main.transform.Translate(new Vector3(0, 0, ges.deltaPinch));  
    }  
}  
```

参数还需要调整，但之前的小目标是实现了，因为容易一下子就把我的小Cube弄不见了，我就添加了一个复位按钮。

摇杆
剩下的我比较感兴趣的也就是摇杆了，毕竟这个东西挺常用，而且，如果你说要让我自己从头做一个，我还是觉得挺懵逼的···
右键EasyTouchControls->Joystick创建一个摇杆
常用的设置可能也就是Type，决定了摇杆是动态的还是静态的，静态就是坐标固定，动态的话当手指离开屏幕，摇杆会消失，手触摸的时候在手指触摸的坐标生成摇杆。
设为动态时可以选择摇杆区域全屏，半屏，还可以自定义区域。
摇杆的值

```
Debug.Log(ETCInput.GetAxis("Horizontal")+","+ ETCInput.GetAxis("Vertical"));  
```



easytouch真的十分强大方便易上手，并且跟UGUI没有什么冲突。更多的功能只有在实际使用中去研究了。



<h1>实例二</h1>
摇杆控制人物移动

创建虚拟摇杆： 
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMTAzMDg0OTU4Nzgy?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMTAzMDg1NDIxNTk3?x-oss-process=image/format,png)
好了，开始使用EasyTouch5实现角色的转向： 
如下设置虚拟摇杆参数： 
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMTAzMDg1NjM3ODAy?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMTAzMDg1NjQ4NzI5?x-oss-process=image/format,png)
实现角色转动，效果： 
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMTAzMDg1NzAzNTg1?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMTAzMDg1NzI4NTUx?x-oss-process=image/format,png)
以上是虚拟摇杆简单的应用，下面我们将实现角色第一人称的移动&转向： 
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMTAzMDg1NzQzNzM4?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMTAzMDg1ODAxMjYx?x-oss-process=image/format,png)
好了，第一人称视角的移动+转向完成！现在通过摇杆控制角色移动，相机会跟随在角色背后。

接着是第三人称视角： 
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMTAzMDg1ODE1ODcz?x-oss-process=image/format,png)


只需要将Turn&Move 选项勾选上，即可控制人物移动&转向。

完成！哈哈，是不是好简单。完全不用接触代码层面，直接通过视图面板设置，这最适合初学者不过。但是没有接触过，或者只是使用过旧版的人可能就一头雾水了。



<h1>实例三</h1>
RPG类的游戏的摇杆控制人物移动
1、以下是EasyTouch插件的使用步骤：
1.import“EasyTouch”资源包

2.创建空物体，命名为EasyTouch(当然你也可以改成其他名字)

3.添加EasyTouch.cs脚本在刚刚创建的空物体（EasyTouch）上

4.选择改物体但不要将BroadcastMessages勾选

5.创建一个新的C#脚本，命名MyFirstTouch，然后添加以下方法

```
void OnEnable(){    
EasyTouch.On_TouchStart += On_TouchStart;    
}    
// Unsubscribe    
void OnDisable(){    
EasyTouch.On_TouchStart -= On_TouchStart;    
}    
// Unsubscribe    
void OnDestroy(){    
EasyTouch.On_TouchStart -= On_TouchStart;    
}    
// Touch start event    
public void On_TouchStart(Gesture gesture){    
Debug.Log( "Touch in " + gesture.position);    
}    
```

6.再创建一个空物体，命名为Receiver

7.将MyFirstTouch脚本添加到空物体Receiver上

8.运行并且点击遥感，会发现控制台打印了当前按下的坐标

2、根据官方的这些提示，自己来做一个属于自己的人物遥感控制

   1.导入EasyTouch3资源包
    2.做好前期准备，包括人物模型、地形的创建
    3.添加JoyStick实例：HedgehogTeam->EasyTouch->Extensions->Add a new Joystick。此时就会在左下角创建虚拟遥感实例          
    ![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMTAzMDkwMTEwODky?x-oss-process=image/format,png)
 5 .创建脚本MoveController.cs用来接收遥感事件控制角色的移动    

```
using UnityEngine;    
using System.Collections;    
    
public class MoveController : MonoBehaviour {    
    
    void OnEnable()    
    {    
        EasyJoystick.On_JoystickMove += OnJoystickMove;    
        EasyJoystick.On_JoystickMoveEnd += OnJoystickMoveEnd;    
    }    
    
    
    //移动摇杆结束    
    void OnJoystickMoveEnd(MovingJoystick move)    
    {    
        //停止时，角色恢复idle    
        if (move.joystickName == "MoveJoystick")    
        {    
            animation.CrossFade("idle");    
        }    
    }    
    
    
    //移动摇杆中    
    void OnJoystickMove(MovingJoystick move)    
    {    
        if (move.joystickName != "MoveJoystick")    
        {    
            return;    
        }    
            
        //获取摇杆中心偏移的坐标    
        float joyPositionX = move.joystickAxis.x;    
        float joyPositionY = move.joystickAxis.y;    
    
    
        if (joyPositionY != 0 || joyPositionX != 0)    
        {    
            //设置角色的朝向（朝向当前坐标+摇杆偏移量）    
           transform.LookAt(new Vector3(transform.position.x + joyPositionX, transform.position.y, transform.position.z + joyPositionY));    
            //移动玩家的位置（按朝向位置移动）    
            transform.Translate(Vector3.forward * Time.deltaTime * 5);    
            //播放奔跑动画    
            animation.CrossFade("run");    
        }    
    }    
}    
```

6.创建点击按钮
点击HedgehogTeam->EasyTouch->Extensions->Create a new Button,会在屏幕右下角创建一个button
然后调节面板参数：

 代码添加跟Unity中Button按钮添加方法类似。



<h1>最后附上研究的EasyTouch3.1.6 API</h1>
EasyTouch.On_TouchStart
函数作用：在点击的时候触发
使用例子：

```
public void On_TouchStart(Gesture gesture)
{
		// Verification that the action on the object
		if (gesture.pickObject == gameObject)
			gameObject.GetComponent<Renderer>().material.color = new Color( Random.Range(0.0f,1.0f),  Random.Range(0.0f,1.0f), Random.Range(0.0f,1.0f));
	}
```
	
EasyTouch.On_TouchDown
函数作用：在按下的时候触发
使用例子：

```
public void On_TouchDown(Gesture gesture)
{
		// Verification that the action on the object
		if (gesture.pickObject == gameObject)
			textMesh.text = "Down since :" + gesture.actionTime.ToString("f2");
	}
```
	

EasyTouch.On_TouchUp
函数作用：在松开的时候触发
使用例子：

```
public void On_TouchUp(Gesture gesture)
{
		// Verification that the action on the object
		if (gesture.pickObject == gameObject)
		{
			gameObject.GetComponent<Renderer>().material.color = new Color( 1f,1f,1f);
			textMesh.text ="Touch Start/Up";
		}
}
```

EasyTouch.On_DoubleTap
函数作用：在双击的时候触发
使用例子：

```
private void On_DoubleTap( Gesture gesture){
		
		// Verification that the action on the object
		if (gesture.pickObject == gameObject){
			
	 		gameObject.GetComponent<Renderer>().material.color = new Color( Random.Range(0.0f,1.0f),  Random.Range(0.0f,1.0f), Random.Range(0.0f,1.0f));
		}
	}
```

EasyTouch.On_SimpleTap
函数作用：在普通的点击时候触发
使用例子：

```
private void On_SimpleTap( Gesture gesture){
		
		// Verification that the action on the object
		if (gesture.pickObject == gameObject){
			gameObject.GetComponent<Renderer>().material.color = new Color( Random.Range(0.0f,1.0f),  Random.Range(0.0f,1.0f), Random.Range(0.0f,1.0f));
		}
	}
```

EasyTouch.On_LongTapStart
函数作用：在长点击的时候触发
使用例子：

```
private void On_LongTapStart( Gesture gesture){
		
		// Verification that the action on the object
		if (gesture.pickObject==gameObject){
			gameObject.GetComponent<Renderer>().material.color = new Color( Random.Range(0.0f,1.0f),  Random.Range(0.0f,1.0f), Random.Range(0.0f,1.0f));
		}
	}
```
	
EasyTouch.On_LongTap
函数作用：在长点击期间的时候触发
使用例子：

```
private void On_LongTap( Gesture gesture){
		
		// Verification that the action on the object
		if (gesture.pickObject==gameObject){
			textMesh.text = gesture.actionTime.ToString("f2");
		}
	
	}
```

EasyTouch.On_LongTapEnd
函数作用：在长点击结束的时候触发
使用例子：

```
private void On_LongTapEnd( Gesture gesture){
		
		// Verification that the action on the object
		if (gesture.pickObject==gameObject){
			gameObject.GetComponent<Renderer>().material.color = Color.white;
			textMesh.text="Long tap";
		}
	
	}
```

EasyTouch.On_Drag
函数作用：在拖拽期间的时候触发
使用例子：

```
void On_Drag(Gesture gesture){
	
		// Verification that the action on the object
		if (gesture.pickObject == gameObject){
			
			// the world coordinate from touch for z=5
			Vector3 position = gesture.GetTouchToWordlPoint(5);
			
			transform.position = position - deltaPosition;
			
			// Get the drag angle
			float angle = gesture.GetSwipeOrDragAngle();
			
			textMesh.text = gesture.swipe.ToString() + " / angle :" + angle.ToString("f2");
		}
	}
```

EasyTouch.On_DragStart
函数作用：在拖拽开始的时候触发
使用例子：

```
void On_DragStart( Gesture gesture){
		
		// Verification that the action on the object
		if (gesture.pickObject == gameObject){
			gameObject.GetComponent<Renderer>().material.color = new Color( Random.Range(0.0f,1.0f),  Random.Range(0.0f,1.0f), Random.Range(0.0f,1.0f));
		
			// the world coordinate from touch for z=5
			Vector3 position = gesture.GetTouchToWordlPoint(5);
			
			
			deltaPosition = position - transform.position;
		}	
	}
```

EasyTouch.On_DragEnd
函数作用：在拖拽结束的时候触发
使用例子：

```
void On_DragEnd(Gesture gesture){
	
		// Verification that the action on the object
		if (gesture.pickObject == gameObject){
			transform.position= new Vector3(3f,1.8f,-5f);
			gameObject.GetComponent<Renderer>().material.color = Color.white;
			textMesh.text="Drag me";
		}
	}
```

EasyTouch.On_TouchStart2Fingers
函数作用：按住Ctrl然后点击的时候触发
使用例子：

```
void On_TouchStart2Fingers( Gesture gesture){
		// Verification that the action on the object
		if (gesture.pickObject == gameObject){	
			gameObject.GetComponent<Renderer>().material.color = new Color( Random.Range(0.0f,1.0f),  Random.Range(0.0f,1.0f), Random.Range(0.0f,1.0f));
		}
	}
```
	
EasyTouch.On_TouchDown2Fingers
函数作用：按住Ctrl然后按下的时候触发
使用例子：

```
void On_TouchDown2Fingers(Gesture gesture){
		
		// Verification that the action on the object
		if (gesture.pickObject == gameObject){	
			textMesh.text = "Down since :" + gesture.actionTime.ToString("f2");
		}
	}
```
	
EasyTouch.On_TouchUp2Fingers
函数作用：按住Ctrl然后松开的时候触发
使用例子：

```
void On_TouchUp2Fingers( Gesture gesture){
		
		// Verification that the action on the object
		if (gesture.pickObject == gameObject){	
			gameObject.GetComponent<Renderer>().material.color = Color.white;
			textMesh.text ="Touch Start/Up";
		}
	}
```

EasyTouch.On_Cancel2Fingers
函数作用：按住Ctrl然后取消的时候触发
使用例子：

```
void On_Cancel2Fingers( Gesture gesture){
		
		// Verification that the action on the object
		if (gesture.pickObject == gameObject){	
			gameObject.GetComponent<Renderer>().material.color = Color.white;
			textMesh.text ="Touch Start/Up";
		}
	}
```

EasyTouch.On_SimpleTap2Fingers
函数作用：按住Ctrl然后普通点击的时候触发
使用例子：

```
void On_SimpleTap2Fingers( Gesture gesture){
		
		
		// Verification that the action on the object
		if (gesture.pickObject == gameObject){	

			gameObject.GetComponent<Renderer>().material.color = new Color( Random.Range(0.0f,1.0f),  Random.Range(0.0f,1.0f), Random.Range(0.0f,1.0f));
		}
	}
```

EasyTouch.On_DoubleTap2Fingers
函数作用：按住Ctrl然后双击的时候触发
使用例子：

```
void On_DoubleTap2Fingers( Gesture gesture){
		
		// Verification that the action on the object
		if (gesture.pickObject == gameObject){
			gameObject.GetComponent<Renderer>().material.color = new Color( Random.Range(0.0f,1.0f),  Random.Range(0.0f,1.0f), Random.Range(0.0f,1.0f));
		}
	}
```

EasyTouch.On_LongTapStart2Fingers
函数作用：按住Ctrl然后长点击的时候触发
使用例子：

```
void On_LongTapStart2Fingers( Gesture gesture){
		// Verification that the action on the object
		if (gesture.pickObject == gameObject){
			gameObject.GetComponent<Renderer>().material.color = new Color( Random.Range(0.0f,1.0f),  Random.Range(0.0f,1.0f), Random.Range(0.0f,1.0f));
		}
	}
```

EasyTouch.On_LongTap2Fingers
函数作用：按住Ctrl然后长点击的期间的时候触发
使用例子：

```
void On_LongTap2Fingers( Gesture gesture){
		// Verification that the action on the object
		if (gesture.pickObject == gameObject){		
			textMesh.text = gesture.actionTime.ToString("f2");
		}
	}
```

EasyTouch.On_LongTapEnd2Fingers 
函数作用：按住Ctrl然后长点击结束的时候触发
使用例子：

```
void On_LongTapEnd2Fingers( Gesture gesture){
		// Verification that the action on the object
		if (gesture.pickObject == gameObject){
			gameObject.GetComponent<Renderer>().material.color = new Color(1f,1f,1f);
			textMesh.text="Long tap";
		}
	}
```

EasyTouch.On_Cancel2Fingers
函数作用：按住Ctrl然后长点击取消的时候触发
使用例子：

```
void On_Cancel2Fingers(Gesture gesture){
		gameObject.GetComponent<Renderer>().material.color = new Color(1f,1f,1f);
		textMesh.text="Long tap";
	}
```

EasyTouch.On_PinchIn
函数作用：按住Next然后拖动的时候触发，缩小
使用例子：

```
// At the 2 fingers touch beginning
	private void On_TouchStart2Fingers( Gesture gesture){
		
		// Verification that the action on the object
		if (gesture.pickObject == gameObject){		
			// disable twist gesture recognize for a real pinch end
			EasyTouch.SetEnableTwist( false);
			EasyTouch.SetEnablePinch( true);
		}
	}
private void On_PinchIn(Gesture gesture){
	
		// Verification that the action on the object
		if (gesture.pickObject == gameObject){
			
			float zoom = Time.deltaTime * gesture.deltaPinch;
		
			Vector3 scale = transform.localScale ;
			transform.localScale = new Vector3( scale.x - zoom, scale.y -zoom, scale.z-zoom);
			
			textMesh.text = "Delta pinch : " + gesture.deltaPinch.ToString();
		}
		
	}
```
	
EasyTouch.On_PinchOut
函数作用：按住Next然后拖动的时候触发，扩大
使用例子：

```
private void On_PinchOut(Gesture gesture){
	
		// Verification that the action on the object
		if (gesture.pickObject == gameObject){
			float zoom = Time.deltaTime * gesture.deltaPinch;
		
			Vector3  scale = transform.localScale ;
			transform.localScale = new Vector3( scale.x + zoom, scale.y +zoom,scale.z+zoom);
			
			textMesh.text = "Delta pinch : " + gesture.deltaPinch.ToString();
		}
	}
```
	
EasyTouch.On_PinchEnd
函数作用：按住Next然后拖动结束的时候触发
使用例子：

```
private void On_PinchEnd(Gesture gesture){
		
		if (gesture.pickObject == gameObject){
			transform.localScale =new Vector3(1.7f,1.7f,1.7f);
			EasyTouch.SetEnableTwist( true);
			textMesh.text="Pinch me";
		}
		
	}
```

EasyTouch.On_Twist
函数作用：按住Next然后旋转的时候触发
使用例子：

```
// At the 2 fingers touch beginning
	void On_TouchStart2Fingers( Gesture gesture){
		
		// Verification that the action on the object
		if (gesture.pickObject == gameObject){			
			EasyTouch.SetEnablePinch( false);
			EasyTouch.SetEnableTwist( true);
		}
	}
	
	// during the txist
	void On_Twist( Gesture gesture){
		
		// Verification that the action on the object
		if (gesture.pickObject == gameObject){	
			transform.Rotate( new Vector3(0,0,gesture.twistAngle));
			textMesh.text = "Delta angle : " + gesture.twistAngle.ToString();
		}
	}
```

EasyTouch.On_TwistEnd
函数作用：按住Next然后旋转结束的时候触发
使用例子：

```
void On_TwistEnd( Gesture gesture){

		// Verification that the action on the object
		if (gesture.pickObject == gameObject){
			EasyTouch.SetEnablePinch( true);
			transform.rotation = Quaternion.identity;
			textMesh.text ="Twist me";
		}
	}

```

EasyTouch.On_Drag2Fingers
函数作用：按住Ctrl然后拖动的时候触发
使用例子：

```
// At the drag beginning
	void On_DragStart2Fingers(Gesture gesture){

		// Verification that the action on the object
		if (gesture.pickObject == gameObject){	
			gameObject.GetComponent<Renderer>().material.color = new Color( Random.Range(0.0f,1.0f),  Random.Range(0.0f,1.0f), Random.Range(0.0f,1.0f));
		
			Vector3 position =  gesture.GetTouchToWordlPoint(  5);
			deltaPosition = position - transform.position;
		}
	}
	
	// During the drag
	void On_Drag2Fingers(Gesture gesture){

		// Verification that the action on the object
		if (gesture.pickObject == gameObject){	
			Vector3 position = gesture.GetTouchToWordlPoint(  5);
			
			transform.position = position - deltaPosition;
			
			float angles =  gesture.GetSwipeOrDragAngle(); 
			
			textMesh.text = gesture.swipe.ToString() + " / angle :" + angles.ToString("f2");
		}
	}
```
	
EasyTouch.On_DragEnd2Fingers
函数作用：按住Ctrl然后拖动结束的时候触发
使用例子：

```
// At the drag end
	void On_DragEnd2Fingers(Gesture gesture){
		
		// Verification that the action on the object
		if (gesture.pickObject == gameObject){			
			transform.position=new Vector3(2.5f,-0.5f,-5f);
			gameObject.GetComponent<Renderer>().material.color = new Color(1f,1f,1f);
			textMesh.text="Drag me";
		}
	}
```

Joystick.On_Manual
函数作用：手动摇杆，参数是Vecotr2
使用例子：

```
Joystick.On_Manual(new Vector2(Input.GetAxis ("Horizontal"), Input.GetAxis("Vertical")));
```

EasyJoystick.On_JoystickMove
函数作用：移动的时候触发
使用例子：

```
void On_JoystickMove( MovingJoystick move){
	
		float angle = move.Axis2Angle(true);
		transform.rotation  = Quaternion.Euler( new Vector3(0,angle,0));
		transform.Translate( Vector3.forward * move.joystickValue.magnitude * Time.deltaTime);	
		
		model.GetComponent<Animation>().CrossFade("Run");

	}
```

EasyJoystick.On_JoystickMoveEnd
函数作用：移动结束的时候触发
使用例子：

```
void On_JoystickMoveEnd (MovingJoystick move)
	{
		model.GetComponent<Animation>().CrossFade("idle");
	}
```

##本文地址
https://blog.csdn.net/q764424567/article/details/78426905
ps：转载请注明
