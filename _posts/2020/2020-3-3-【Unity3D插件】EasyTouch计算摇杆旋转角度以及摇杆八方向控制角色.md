---
layout: post
category: Unity3D-Plugin
title: 【Unity3D插件】EasyTouch计算摇杆旋转角度以及摇杆八方向控制角色
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
在写第三人称控制的时候，一开始在电脑测试是用WASD控制角色
后来需要发布到手机上，于是就加了一个摇杆
键盘控制角色的代码已经写好了，角色八方向移动

## 二、传统控制思路

```csharp
 //当摇杆处于移动状态时，角色开始奔跑
  void OnJoystickMove(MovingJoystick move)
  {
    if (move.joystickName != "EasyJoystick")
    {
       return;
    }
   //获取摇杆偏移量
   float joyPositionX = move.joystickAxis.x;
   float joyPositionY = move.joystickAxis.y;
   if (joyPositionY != 0 || joyPositionX != 0)
   {
      //设置角色的朝向（朝向当前坐标+摇杆偏移量）
      transform.LookAt(new Vector3(transform.position.x + joyPositionX, transform.position.y, transform.position.z + joyPositionY));
       //移动玩家的位置（按朝向位置移动）
      transform.Translate(Vector3.forward * Time.deltaTime * 7.5F);
      //播放奔跑动画
       animation.CrossFade("Run");
     }
  }
```
## 三、控制摇杆角度
如果要按照摇杆传统思路控制角色，在重新写控制角色代码的话非常麻烦，所以我就通过计算摇杆旋转角度来判断当前摇杆处于哪个方向
ok，现在我们开始来敲代码
首先，我们来调试观察一下摇杆的x轴、y轴的返回值
//移动摇杆中  

```csharp
void OnJoystickMove(MovingJoystick move)
{
      Debug.Log(move.joystickAxis.x + "," + move.joystickAxis.y);
}
```
调试结果为：
左：x = -1，y = 0；顺时针旋转X逐渐变大，Y逐渐变大
上：x = 0，y = 1；顺时针旋转X逐渐变大，Y逐渐变小
右：x = 1，y = 0；顺时针旋转X逐渐变小，Y逐渐变小
下：x = 0，y = -1；顺时针旋转X逐渐变小，Y逐渐变大
我们把摇杆底图看成是两个半圆，上半圆和下半圆
![在这里插入图片描述](https://img-blog.csdn.net/20151223160823558?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
那么：
X轴移动到左边时，X = -1；X轴移动到右边时：X = 1；X轴从左转到右旋转了180度
Y轴移动到左边时，Y = 0；Y轴移动到右边时：Y = 0；Y轴从左转到右旋转了180度
你如果直接看我的调试结果肯定有点晕，建议边调试边参考我的调试结果，这样才能理解
如果我们要计算当前摇杆在左上角旋转的度数怎么计算呢？
读过小学的人都可以做，只是看见摇杆返回的-1和0脑袋被绕迷糊了，我也一样，弄了好半天才弄好

当摇杆移动到左时，为0度、360度(因为360度为一圈，已经绕回远点了)
当摇杆移动到上时，为90度
当摇杆移动到右时，为180度
当摇杆移动到下时，为270度

既然知道是多少度这就好办多了
得出公式：
当X轴在右时为1，也就是X轴为180度，则：1 * 90 + 90 = 180
当前X轴旋转角度为：X轴返回值 * 90度 + 90度

你以为这就完事了吗？还高兴得太早，用这个公式计算只能得到上半圆的旋转角度
现在我们要获取下半圆旋转角度，然后用上半圆旋转角度 + 下半圆旋转角度 = 当前旋转角度
当摇杆移动到下半圆时我们怎么计算旋转角度呢？
我们已经知道Y轴在左边为0，在右边为0，在下边为-1，继续用计算X轴的公式

Y左：0 * 90 + 90 = 90
Y上：1 * 90 + 90 = 180
Y下：-1 * 90 + 90 = 0
Y右：0 * 90 + 90 = 90
X左：-1 * 90 + 90 = 0
X上：0 * 90 + 90 = 90
X下：0 * 90 + 90 = 90
X右：1 * 90 + 90 = 180

从计算结果中可以得出结论
当Y轴小于90度时，摇杆就处于下半圆
当Y轴小于90度时且X小于90度时为左下：270度 + Y轴旋转角度
当Y轴小于90度时且X大于90度时为右下：180度 + Y轴旋转角度
思路搞定了，开始敲代码，代码不多，我直接贴上来了，看完上诉文字相信你已经知道这些代码是怎么回事了

```csharp
/// 计算摇杆角度 <summary>
/// 计算摇杆角度
/// </summary>
/// <param name="_joyPositionX">摇杆X轴</param>
/// <param name="_joyPositionY">摇杆Y轴</param>
/// <returns>返回当前摇杆旋转多少度</returns>
private float CalculaAngle(float _joyPositionX, float _joyPositionY)
{
   float currentAngleX = _joyPositionX * 90f + 90f;//X轴 当前角度
   float currentAngleY = _joyPositionY * 90f + 90f;//Y轴 当前角度
   //下半圆
   if (currentAngleY < 90f)
   {
        if (currentAngleX < 90f)
        {
             return 270f + currentAngleY;
        }
        else if (currentAngleX > 90f)
        {
              return 180f + (90f - currentAngleY);
        }
     }
     return currentAngleX;
}
```
ok，现在知道当前摇杆旋转了多少度，我们可以轻松的用角度来判断当前移动方向了

用键盘控制时：
A = 左
WA = 左上
W = 上
WD = 右上
D = 右
SD = 右下
S = 下
SA = 左下

当摇杆角度为0度，往左
当摇杆角度为90度，往上
当摇杆角度为180度，往右
那我们肯定不能这样写啊，你能确定玩家操作摇杆那么精确啊？
因为我这里控制角色是八方向，所以：360 / 8 = 45
每个方向有45度可触发，那么得出以下解决方案：
上：当前角度 <= 90 + 45 / 2 = 112.5 && 当前角度 >= 90 - 45 / 2 = 67.5
如法炮制，得出以下代码：

```csharp
float currentAngle = CalculaAngle(joyPositionX, joyPositionY);

if (currentAngle <= 22.5f && currentAngle >= 0f || currentAngle <= 360f && currentAngle >= 337.5f)//0;左
    CurrentDire = "A";
else if (currentAngle <= 67.5f && currentAngle >= 22.5f)//45;左上
    CurrentDire = "WA";
else if (currentAngle <= 112.5f && currentAngle >= 67.5f)//90;上
    CurrentDire = "W";
else if (currentAngle <= 157.5f && currentAngle >= 112.5f)//135;右上
    CurrentDire = "WD";
else if (currentAngle <= 202.5f && currentAngle >= 157.5f)//180;右
    CurrentDire = "D";
else if (currentAngle <= 247.5f && currentAngle >= 202.5f)//225;右下
    CurrentDire = "SD";
else if (currentAngle <= 292.5f && currentAngle >= 247.5f)//270;下
    CurrentDire = "S";
else if (currentAngle <= 337.5f && currentAngle >= 292.5f)//315;左下
    CurrentDire = "SA";
```

大功告成，赶紧发布到手机上跑一下试试，这就是我想要的摇杆八方向操作效果
