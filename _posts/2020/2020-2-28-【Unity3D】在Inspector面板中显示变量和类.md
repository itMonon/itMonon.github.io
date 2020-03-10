---
layout: post
category: Unity3D-Daily
title: 【Unity3D】在Inspector面板中显示变量和类
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
Unity有一个强大的特性，它可以帮助我们在没有任何编程的情况下修改Inspector面板中的值。本文介绍了我们需要知道的所有信息，以便在Unity Inspector面板中显示我们的变量和自定义类。

## 二、显示变量
**变量**
让我们创建一个名为“Test.cs”的C#脚本，其中包含一个int变量：

```csharp
using UnityEngine;
using System.Collections;

public class Test : MonoBehaviour {
    int something = 0;

    // Use this for initialization
    void Start () {
        
    }
        
    // Update is called once per frame
    void Update () {
        
    }
}
```
现在我们通过顶部菜单创建一个新的GameObject(游戏对象->创建空)并将Test.cs脚本添加到GameObject。Inspector面板现在看上去是这样的：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS9zaG93LXZhcmlhYmxlcy1hbmQtY2xhc3Nlcy1pbi1pbnNwZWN0b3IvaW5zcGVjdG9yX2VtcHR5LnBuZw)
我们可以看到它上面有Test.cs脚本，但是我们没有看到某物一点变数都没有。

**公共变量**
让我们通过添加另一个变量来修改我们的脚本，但是这次用public前缀。在编程语言中，public意味着其他类也可以看到这个值。在Unity public中，也意味着变量显示在Inspector面板中。

修改后的Test.cs脚本：

```csharp
using UnityEngine;
using System.Collections;

public class Test : MonoBehaviour {
    int something = 0;
    public int i_am_public = 0;

    // Use this for initialization
    void Start () {
        
    }
        
    // Update is called once per frame
    void Update () {
        
    }
}
```
保存了它之后，下面是Inspector面板现在的样子：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS9zaG93LXZhcmlhYmxlcy1hbmQtY2xhc3Nlcy1pbi1pbnNwZWN0b3IvaW5zcGVjdG9yX3ZhcmlhYmxlLnBuZw)
现在，我们可以轻松地修改变量，而无需再次打开脚本，也无需进行任何编程。

## 三、显示类
**公共类+变量**

我们的测试脚本现在应该有一个Address变量。假设我们也可以在其他脚本中使用一个Address，我们就可以为它创建一个完整的类。让我们开始吧：

```csharp
using UnityEngine;
using System.Collections;

public class Test : MonoBehaviour {
    int something = 0;
    public int i_am_public = 0;
    
    public Address address;

    // Use this for initialization
    void Start () {
        
    }
        
    // Update is called once per frame
    void Update () {
        
    }
}

public class Address {
    public string country = "";
    public string city = "";
    public string street = "";
    public int number = 0;
}
```
现在，如果我们再次保存并查看Inspector面板，下面是我们所看到的：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS9zaG93LXZhcmlhYmxlcy1hbmQtY2xhc3Nlcy1pbi1pbnNwZWN0b3IvaW5zcGVjdG9yX3ZhcmlhYmxlLnBuZw)
即使我们设置了Address公众变量在我们的考试课上，我们仍然看不到它。原因是它不是像“public class Test：MonoBehaviour”那样的Unity类(每个组件都需要：MonoBehaviour，但一个简单的类没有理由是：MonoBehaviour).

大多数人在这里停下来，认为不可能在Inspector面板显示我们的Address，但实际上是这样。

**类显示在Inspector面板中**
诀窍是：如果一个类应该显示在Inspector面板中，我们只需通过编写[System.erialable]在类声明之上

下面是我们修改的脚本：

```csharp
using UnityEngine;
using System.Collections;

public class Test : MonoBehaviour {
    int something = 0;
    public int i_am_public = 0;
    
    public Address address;

    // Use this for initialization
    void Start () {
        
    }
        
    // Update is called once per frame
    void Update () {
        
    }
}

[System.Serializable]
public class Address {
    public string country = "";
    public string city = "";
    public string street = "";
    public int number = 0;
}
```
现在，如果我们保存它并再次查看Inspector面板，这就是我们得到的：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS9zaG93LXZhcmlhYmxlcy1hbmQtY2xhc3Nlcy1pbi1pbnNwZWN0b3IvaW5zcGVjdG9yX2NsYXNzLnBuZw)
就这么简单！
