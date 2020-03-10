---
layout: post
category: Unity3D-Daily
title: 【Unity3D日常】Unity3d与串口通信程序的开发
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
记得以前写了一篇Unity3D与串口通信程序的开发的文章，主要讲的是如何用Unity3D程序给串口发送数据，具体可以查看这篇文章https://blog.csdn.net/q764424567/article/details/78710739
但是最近有小伙伴问我如何接收串口程序，今天就再分享一下比较完整的Unity开发的串口通信程序吧，包括发送与接收数据，绑定串口号等。



## 二、文章链接
[Unity3d 与串口的通信程序的开发，软件硬件结合](https://blog.csdn.net/q764424567/article/details/78710739)
[【Unity3D日常】Unity3d与串口通信程序的开发](https://blog.csdn.net/q764424567/article/details/101053821)

## 三、正文
1、先做一个UI界面吧
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190920105823954.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190920105848810.png)
做的比较简单，就不多数哦了


2、代码

```csharp
using UnityEngine;
using System.IO.Ports;
using System.Text;
using UnityEngine.UI;

public class SerialPortTest : MonoBehaviour
{
    private SerialPort sp = new SerialPort();
    public Text m_TextSendDataPar;
    public Text m_TextShowData;

    // Use this for initialization
    void Start()
    {
        //打开串口
        Init("COM1", 9600, Parity.None, 8, StopBits.None);
    }

    //发送数据按钮
    public void Btn_SendData()
    {
        Data_Send(m_TextSendDataPar.ToString());
    }

    //初始化串口类
    public void Init(string _portName,int _baudRate,Parity _parity,int dataBits,StopBits _stopbits)
    {
        sp = new SerialPort(_portName, _baudRate, _parity, dataBits, _stopbits);//绑定端口
        sp.DataReceived += new SerialDataReceivedEventHandler(Data_Received);//订阅委托
    }

    //接收数据
    private void Data_Received(object sender, SerialDataReceivedEventArgs e)
    {
        byte[] ReDatas = new byte[sp.BytesToRead];
        sp.Read(ReDatas, 0, ReDatas.Length);//读取数据
        this.Data_Show(ReDatas);//显示数据
    }

    /// <summary>
    /// 显示数据
    /// </summary>
    /// <param name="data">字节数组</param>
    public void Data_Show(byte[] data)
    {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < data.Length; i++)
        {
            sb.AppendFormat("{0:x2}" + "", data[i]);
        }
        Debug.Log(sb.ToString());
        m_TextShowData.text = sb.ToString();
    }

    //发送数据
    public void Data_Send(string _parameter)
    {
        sp.Open();
        sp.WriteLine(_parameter);
        sp.Close();
    }
}

```
3、绑定参数
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190920110011121.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019092011003712.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
4、找不到命名空间的问题

在unity 引用System.IO.Ports 却发现引用不到 查了一下才看到 要在[Edit->Project Settings->Player]下，修改[Other Settings]下的[Optimization]的[API Compatibility Level]为[.NET 2.0]（默认为[.NET 2.0 Subset]。才能找到
*PS:感谢单曲循环小盆友的提醒
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190920110455805.png)






OK了。。。小伙们可以试试了 
对啦，那个数据只是接收，然后显示了，具体要怎么解析，获取端口号啥的就让小伙伴们自己来吧。。
