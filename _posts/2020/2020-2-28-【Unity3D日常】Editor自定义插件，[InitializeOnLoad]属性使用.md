---
layout: post
category: Unity3D-Daily
title: 【Unity3D日常】Editor自定义插件，[InitializeOnLoad]属性使用
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
以前就介绍过自定义组件，文章链接在此：
Unity3D Editor自定义窗口、自定义组件、Inspector、菜单等等https://blog.csdn.net/q764424567/article/details/80908614

今天又学到了一个新属性InitializeOnLoad
就记录一下好啦

## 二、使用
这个属性主要作用是启动Unity的时候运行编辑器脚本，大白话就是，我想要项目一打开就运行我的脚本，比如使用我的插件，一打开就让你知道我插件已经更新了。
看一下它官方的例子，就知道它需要一个静态的构造函数


```csharp
using UnityEngine;
using UnityEditor;
 
[InitializeOnLoad]
public class Startup {
    static Startup()
    {
        Debug.Log("Up and running");
    }
}
```


## 三、例子
例子一：项目加载完毕就调用脚本
```csharp
using UnityEditor;
using UnityEngine;
 
[InitializeOnLoad]
class MyClass
{
    static MyClass ()
    {
        EditorApplication.update += Update;
    }
 
    static void Update ()
    {
        Debug.Log("Updating");
    }
}
```


例子二：加载完窗口，就关闭调用
```csharp
using UnityEditor;
using UnityEngine;
 
[InitializeOnLoad]
class MyClass
{
    static MyClass ()
    {
        EditorApplication.update += Update;
    }
 
    static void Update ()
    {
    	bool isSuccess = EditorApplication.ExecuteMenuItem("Welcome");
        if (isSuccess) EditorApplication.update -= Update;
    }
}
```

例子三：窗口自定义

```csharp
using UnityEditor;
using UnityEngine;


[InitializeOnLoad]
public class MyClass
{
    static MyClass()
    {
        bool hasKey = PlayerPrefs.HasKey("welcome");
        if (hasKey == false)
        {
            PlayerPrefs.SetInt("welcome", 1);
            WelcomeScreen.ShowWindow();
        }
    }
}

public class WelcomeScreen : EditorWindow
{
    private Texture mSamplesImage;
    private Rect imageRect = new Rect(30f, 90f, 350f, 350f);
    private Rect textRect = new Rect(15f, 15f, 380f, 100f);

    public void OnEnable()
    {
        mSamplesImage = LoadTexture("wechat.jpg");
    }

    Texture LoadTexture(string name)
    {
        string path = "Assets/Editor/";
        return (Texture)AssetDatabase.LoadAssetAtPath(path + name, typeof(Texture));
    }

    public void OnGUI()
    {
        GUIStyle style = new GUIStyle();
        style.fontSize = 14;
        style.normal.textColor = Color.white;
        GUI.Label(textRect, "欢迎扫一扫，关注微信号\n这个页面只会显示一次", style);
        GUI.DrawTexture(imageRect, mSamplesImage);
    }

    public static void ShowWindow()
    {
        WelcomeScreen window = GetWindow<WelcomeScreen>(true, "Hello");
        window.minSize = window.maxSize = new Vector2(410f, 470f);
        DontDestroyOnLoad(window);
    }
}

```

