---
layout: post
category: Unity3D-Daily
title: 【Unity3D】协程——线程（Thread）和协程（Coroutine）
tagline: by 恬静的小魔龙
tag: Unity3D
---

很多小伙伴会分不清线程和协程的区别，这也是很多初级程序面试经常遇到的题目，在此特出一个系列，专门讲解Unity协程。
使用协程的作用一共有两点：
    1.  延时（等待）一段时间执行代码；
    2. 等某个操作完成之后再执行后面的代码。总结起来就是一句话：控制代码在特定的时机执行。
很多初学者，都会下意识地觉得协程是异步执行的，都会觉得协程是C# 线程的替代品，是Unity不使用线程的解决方案。
        所以首先，请你牢记：协程不是线程，也不是异步执行的。协程和 MonoBehaviour 的 Update函数一样也是在MainThread中执行的。使用协程你不用考虑同步和锁的问题。
 下面请看协程的开启方法：
public Coroutine StartCoroutine(IEnumerator routine);1.根据协程定义的方法体来开启2.根据方法体对应的协程引用来开启。
public Coroutine StartCoroutine(string methodName, object value = null);根据协程定义的方法名字符串开启协程。
停止协程的方法：
public void StopCoroutine(string methodName);根据协程的方法名，来停止协程。
public void StopCoroutine(IEnumerator routine);根据保存的协程引用，来停止协程。
public void StopAllCoroutines();停止在本Behavior上运行的所有协程。（注意：本Behavior）

参考一下代码的例子： copy

```
using UnityEngine;  
using System.Collections;  
  public class Example : MonoBehaviour {    
  // 保持一个执行脚本的引用  
  private IEnumerator coroutine;  

    void Start () {  
        print("Starting " + Time.time);  
        //方法1：保存协程的引用，开启协程；  
        {  
            coroutine = WaitAndPrint(3.0f);  
            StartCoroutine(coroutine);  
        }  
        //方法2：把协程的方法作为字符串，开启协程  
        {  
            StartCoroutine("WaitAndPrint",3.0f);  
        }  
        print("Done " + Time.time);  
    }  
  
    // 每隔3秒，打印一次  
    // 由yield来挂起(暂停)函数  
    public IEnumerator WaitAndPrint(float waitTime) {  
        while (true) {  
            yield return new WaitForSeconds(waitTime);  
            print("WaitAndPrint " + Time.time);  
        }  
    }  
      
    void Update () {  
        if (Input.GetKeyDown("space")){  
             //方法2：利用保存的协程变量 停止  
            {  
                StopCoroutine(coroutine);  
            }  
             //方法2：字符串 停止协程  
            {  
                StopCoroutine("WaitAndPrint");  
            }  
            //方法2：停止本脚本下运行的所有协程  
            {  
                StopAllCoroutines();  
            }  
            print("Stopped " + Time.time);  
        }  
    }  
}  
```
