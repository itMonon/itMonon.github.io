---
layout: post
category: Unity3D-Daily
title: 【Unity3D】Player角色移动控制脚本
tagline: by 恬静的小魔龙
tag: Unity3D
---

## Unity3D Player角色移动控制脚本
在Unity3D中，有多种方式可以改变物体的坐标，实现移动的目的，其本质是每帧修改物体的position。我针对角色移动做了一下盘点,并给出了一些常用API的脚本,每个脚本均已测试可用

### 1.通过Transform组件移动物体
Transform组件用于描述物体在空间中的状态，它包括位置(position)，旋转(rotation)和缩放(scale)。 其实所有的移动都会导致position的改变，这里所说的通过Transform组件来移动物体，指的是直接操作Transform来控制物体的位置(position)。
**Transform.Translate**
该方法可以将物体从当前位置，移动到指定位置，并且可以选择参照的坐标系。 当需要进行坐标系转换时，可以考虑使用该方法以省去转换坐标系的步骤。

```csharp
	public float m_speed = 5f;
	//Translate移动控制函数
    void MoveControlByTranslate()
    {
        if (Input.GetKey(KeyCode.W)|Input.GetKey(KeyCode.UpArrow)) //前
        {
            this.transform.Translate(Vector3.forward*m_speed*Time.deltaTime);
        }
        if (Input.GetKey(KeyCode.S) | Input.GetKey(KeyCode.DownArrow)) //后
        {
            this.transform.Translate(Vector3.forward *- m_speed * Time.deltaTime);
        }
        if (Input.GetKey(KeyCode.A) | Input.GetKey(KeyCode.LeftArrow)) //左
        {
            this.transform.Translate(Vector3.right *-m_speed * Time.deltaTime);
        }
        if (Input.GetKey(KeyCode.D) | Input.GetKey(KeyCode.RightArrow)) //右
        {
            this.transform.Translate(Vector3.right * m_speed * Time.deltaTime);
        }
    }
```
或者

```csharp
	 //Translate移动控制函数
    void MoveControlByTranslateGetAxis()
    {
        float horizontal = Input.GetAxis("Horizontal"); //A D 左右
        float vertical = Input.GetAxis("Vertical"); //W S 上 下

        transform.Translate(Vector3.forward * vertical * m_speed * Time.deltaTime);//W S 上 下
        transform.Translate(Vector3.right * horizontal * m_speed * Time.deltaTime);//A D 左右
    }
```

**Vector3.Lerp, Vector3.Slerp, Vector3.MoveTowards**
Vector3既可以表示三维空间中的一个点，也可以表示一个向量。这三个方法均为插值方法，Lerp</strong>为线性插值，Slerp为球形插值，MoveTowards在Lerp的基础上增加了限制最大速度功能。 当需要从指定A点移动到B点时,可以考虑时候这些方法。
**Vector3.SmoothDamp**
该方法是可以平滑的从A逐渐移动到B点，并且可以控制速度，最常见的用法是相机跟随目标。
**Transform.position**
有时重新赋值position能更快实现我们的目标。
### 2.通过Rigidbody组件移动物体
Rigidbody组件用于模拟物体的物理状态，比如物体受重力影响，物体被碰撞后的击飞等等。
注意：关于Rigidbody的调用均应放在FixedUpdate方法中，该方法会在每一次执行物理模拟前被调用。
**Rigidbody.velocity**
设置刚体速度可以让物体运动并且忽略静摩擦力，这会让物体快速从静止状态进入运动状态。

```csharp
 	//Velocity移动控制函数
    void MoveControlByVelocity()
    {
        float horizontal = Input.GetAxis("Horizontal"); //A D 左右
        float vertical = Input.GetAxis("Vertical"); //W S 上 下
        //这个必须分开判断 因为一个物体的速度只有一个
        if (Input.GetKey(KeyCode.W) | Input.GetKey(KeyCode.S))
        {
            m_rigidbody.velocity = Vector3.forward * vertical * m_speed;
        }
        if (Input.GetKey(KeyCode.A)|Input.GetKey(KeyCode.D))
        {
            m_rigidbody.velocity = Vector3.right * horizontal * m_speed;
        }   
    }
```

**Rigidbody.AddForce**
给刚体添加一个方向的力，这种方式适合模拟物体在外力的作用下的运动状态。

```csharp
	//AddForce移动控制函数
    void MoveControlByAddForce()
    {
        float horizontal = Input.GetAxis("Horizontal"); //A D 左右
        float vertical = Input.GetAxis("Vertical"); //W S 上 下

        m_rigidbody.AddForce(Vector3.forward * vertical * m_speed);
        m_rigidbody.AddForce(Vector3.right * horizontal * m_speed);  

    }
```

**Rigidbody.MovePosition**
刚体受到物理约束的情况下，移动到指定点。
### 3.通过CharacterController组件移动物体
CharacterController用于控制第一人称或第三人称角色的运动，使用这种方式可以模拟人的一些行为，比如限制角色爬坡的最大斜度,步伐的高度等。
**CharacterController.SimpleMove**
用于模拟简单运动，并且自动应用重力，返回值表示角色当前是否着地。

```csharp
//SimpleMove移动控制函数 角色控制器
    void MoveControlBySimpleMove()
    {
        float horizontal = Input.GetAxis("Horizontal"); //A D 左右
        float vertical = Input.GetAxis("Vertical"); //W S 上 下

        m_character.SimpleMove(transform.forward * vertical * m_speed); 
    }
```

**CharacterController.Move**
模拟更复杂的运动,重力需要通过代码实现，返回值表示角色与周围的碰撞信息。

```csharp
	//Move移动控制函数 角色控制器
    void MoveControlByMove()
    {
        float horizontal = Input.GetAxis("Horizontal"); //A D 左右
        float vertical = Input.GetAxis("Vertical"); //W S 上 下
        float moveY = 0; m_gravity=10f;
        moveY-= m_gravity * Time.deltaTime;//重力

        m_character.Move(new Vector3(horizontal, moveY, vertical) * m_speed * Time.deltaTime);
    }
```
    

       
