---
layout: post
category: Unity3D-Daily
title: 【Unity3D】Unity3d 与串口的通信程序的开发，软件硬件结合
tagline: by 恬静的小魔龙
tag: Unity3D
---


<!DOCTYPE html>
<html lang="zh-cn">
<head>
</head>
<body>
<div id="main">
	<div id="mainContent">
	<div class="forFlow">
		
<div id="post_detail">
<!--done-->
<div id="topics">
	<div class = "post">
		<h1 class = "postTitle">
		<a>一、c#实现串口通信程序的开发</a>
		</h1>
		<div class="clear"></div>
		<div class="postBody">
			<div id="cnblogs_post_body"><h1>C#串口介绍以及简单串口通信程序设计实现</h1>
<p>简单的串口通信工具，基于C#应用程序WinFrom实现</p>
<h2>串口介绍</h2>
<p>　　串行接口简称串口，也称串行通信接口或串行通讯接口（通常指COM接口），是采用串行通信方式的扩展接口。（至于再详细，自己百度）</p>
<h2>串口应用：</h2>
<p>　　工业领域使用较多，比如:数据采集，设备控制等等，好多都是用串口通信来实现！你要是细心的话，你会发现，目前家用国网智能电能表就具备RS485通信总线（串行总线的一种）与RS232可以相互转化（当然一般，非专业的谁也不会闲的蛋疼，趴电表上瞎看,最多也就看看走了多少度电）</p>
<h2>RS232 DB9介绍：</h2>
<h3>1.示意图</h3>
<p><img src="http://images2015.cnblogs.com/blog/1070330/201703/1070330-20170325183526783-1521639062.png" alt="" /></p>
<h3>2.针脚介绍：</h3>
<ol>
<li>&nbsp;载波检测(DCD)&nbsp;</li>
<li>&nbsp;接受数据(RXD)&nbsp;</li>
<li>&nbsp;发出数据(TXD)&nbsp;</li>
<li>&nbsp;数据终端准备好(DTR)&nbsp;</li>
<li>&nbsp;信号地线(SG)</li>
<li>&nbsp;数据准备好(DSR)</li>
<li>&nbsp;请求发送(RTS)</li>
<li>&nbsp;清除发送(CTS)</li>
<li>&nbsp;振铃指示(RI)</li>
</ol>
<h3>3.实物图：</h3>
<p>以下是一个usb转串口线：这个头就是一个公头，另一端是一个usb口</p>
<p><img src="http://images2015.cnblogs.com/blog/1070330/201703/1070330-20170325185355893-828946816.png" alt="" /></p>
<h2>笨小孩串口工具运行图：</h2>
<h3>1.开启程序</h3>
<p><img src="http://images2015.cnblogs.com/blog/1070330/201703/1070330-20170325194243127-976559948.png" alt="" /></p>
<h3>2.发送一行字符串HelloBenXH，直接将针脚的发送和接收链接起来就可以测试了（针脚2&nbsp;接受数据(RXD) 和3 发出数据(TXD)）直接链接,</h3>
<p><img src="http://images2015.cnblogs.com/blog/1070330/201703/1070330-20170325194029033-795198649.png" alt="" /></p>
<h2>C#代码实现：采用SerialPort</h2>
<h3>1.实例化一个SerialPort</h3>
<div class="cnblogs_code">

```
private SerialPort ComDevice = new SerialPort();
```

<pre></pre>
</div>
<h3>2.初始化参数绑定接收数据事件</h3>

  

```csharp
public void init()
{
      btnSend.Enabled = false;
      cbbComList.Items.AddRange(SerialPort.GetPortNames());
      if (cbbComList.Items.Count > 0)
      {
           cbbComList.SelectedIndex = 0;
      }
      cbbBaudRate.SelectedIndex = 5;
      cbbDataBits.SelectedIndex = 0;
      cbbParity.SelectedIndex = 0; 
      cbbStopBits.SelectedIndex = 0;
      pictureBox1.BackgroundImage = Properties.Resources.red;
      ComDevice.DataReceived += new SerialDataReceivedEventHandler(Com_DataReceived);//绑定事件      
}
```



<h3>3.打开串口button事件</h3>

```csharp
/// <summary>
/// 打开串口
/// </summary>
/// <param name="sender"></param>
/// <param name="e"></param>
private void btnOpen_Click(object sender, EventArgs e)
{
    if (cbbComList.Items.Count <= 0)
    {
        MessageBox.Show("没有发现串口,请检查线路！");
        return;
    }

    if (ComDevice.IsOpen == false)
    {
        ComDevice.PortName = cbbComList.SelectedItem.ToString();
        ComDevice.BaudRate = Convert.ToInt32(cbbBaudRate.SelectedItem.ToString());
        ComDevice.Parity = (Parity)Convert.ToInt32(cbbParity.SelectedIndex.ToString());
        ComDevice.DataBits = Convert.ToInt32(cbbDataBits.SelectedItem.ToString());
        ComDevice.StopBits = (StopBits)Convert.ToInt32(cbbStopBits.SelectedItem.ToString());
        try
        {
            ComDevice.Open();
            btnSend.Enabled = true;
        }
        catch (Exception ex)
        {
            MessageBox.Show(ex.Message, "错误", MessageBoxButtons.OK, MessageBoxIcon.Error);
            return;
        }
        btnOpen.Text = "关闭串口";
        pictureBox1.BackgroundImage = Properties.Resources.green;
    }
    else
    {
        try
        {
            ComDevice.Close();
            btnSend.Enabled = false;
        }
        catch (Exception ex)
        {
            MessageBox.Show(ex.Message, "错误", MessageBoxButtons.OK, MessageBoxIcon.Error);
        }
        btnOpen.Text = "打开串口";
        pictureBox1.BackgroundImage = Properties.Resources.red;
    }

    cbbComList.Enabled = !ComDevice.IsOpen;
    cbbBaudRate.Enabled = !ComDevice.IsOpen;
    cbbParity.Enabled = !ComDevice.IsOpen;
    cbbDataBits.Enabled = !ComDevice.IsOpen;
    cbbStopBits.Enabled = !ComDevice.IsOpen;
}
```

<h3>4.发送数据</h3>

```csharp
/// <summary>
/// 发送数据
/// </summary>
/// <param name="sender"></param>
/// <param name="data"></param>
public bool SendData(byte[] data)
{
    if (ComDevice.IsOpen)
    {
        try
        {
            ComDevice.Write(data, 0, data.Length);//发送数据
            return true;
        }
        catch (Exception ex)
        {
            MessageBox.Show(ex.Message, "错误", MessageBoxButtons.OK, MessageBoxIcon.Error);
        }
    }
    else
    {
        MessageBox.Show("串口未打开", "错误", MessageBoxButtons.OK, MessageBoxIcon.Error);
    }
    return false;
}

/// <summary>
/// 发送数据button事件
/// </summary>
/// <param name="sender"></param>
/// <param name="e"></param>
private void btnSend_Click(object sender, EventArgs e)
{
    byte[] sendData = null;

    if (rbtnSendHex.Checked)
    {
        sendData = strToHexByte(txtSendData.Text.Trim());
    }
    else if (rbtnSendASCII.Checked)
    {
        sendData = Encoding.ASCII.GetBytes(txtSendData.Text.Trim());
    }
    else if (rbtnSendUTF8.Checked)
    {
        sendData = Encoding.UTF8.GetBytes(txtSendData.Text.Trim());
    }
    else if (rbtnSendUnicode.Checked)
    {
        sendData = Encoding.Unicode.GetBytes(txtSendData.Text.Trim());
    }
    else
    {
        sendData = Encoding.ASCII.GetBytes(txtSendData.Text.Trim());
    }

    if (this.SendData(sendData))//发送数据成功计数
    {
        lblSendCount.Invoke(new MethodInvoker(delegate
    {
        lblSendCount.Text = (int.Parse(lblSendCount.Text) + txtSendData.Text.Length).ToString();
    }));
    }
    else
    {

    }

}

/// <summary>
/// 字符串转换16进制字节数组
/// </summary>
/// <param name="hexString"></param>
/// <returns></returns>
private byte[] strToHexByte(string hexString)
{
    hexString = hexString.Replace(" ", "");
    if ((hexString.Length % 2) != 0)
        hexString += " ";
    byte[] returnBytes = new byte[hexString.Length / 2];
    for (int i = 0; i < returnBytes.Length; i++)
        returnBytes[i] = Convert.ToByte(hexString.Substring(i * 2, 2).Replace(" ", ""), 16);
    return returnBytes;
}
```

<h3>5.接收和数据输出</h3>

```csharp
/// <summary>
/// 接收数据
/// </summary>
/// <param name="sender"></param>
/// <param name="e"></param>
private void Com_DataReceived(object sender, SerialDataReceivedEventArgs e)
{
    byte[] ReDatas = new byte[ComDevice.BytesToRead];
    ComDevice.Read(ReDatas, 0, ReDatas.Length);//读取数据
    this.AddData(ReDatas);//输出数据
}

/// <summary>
/// 添加数据
/// </summary>
/// <param name="data">字节数组</param>
public void AddData(byte[] data)
{
    if (rbtnHex.Checked)
    {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < data.Length; i++)
        {
            sb.AppendFormat("{0:x2}" + " ", data[i]);
        }
        AddContent(sb.ToString().ToUpper());
    }
    else if (rbtnASCII.Checked)
    {
        AddContent(new ASCIIEncoding().GetString(data));
    }
    else if (rbtnUTF8.Checked)
    {
        AddContent(new UTF8Encoding().GetString(data));
    }
    else if (rbtnUnicode.Checked)
    {
        AddContent(new UnicodeEncoding().GetString(data));
    }
    else
    { }

    lblRevCount.Invoke(new MethodInvoker(delegate
    {
        lblRevCount.Text = (int.Parse(lblRevCount.Text) + data.Length).ToString();
    }));
}


/// <summary>
/// 输入到显示区域
/// </summary>
/// <param name="content"></param>
private void AddContent(string content)
{
    this.BeginInvoke(new MethodInvoker(delegate
    {
        if (chkAutoLine.Checked && txtShowData.Text.Length > 0)
        {
            txtShowData.AppendText("\r\n");
        }
        txtShowData.AppendText(content);
    }));
}

```

<h3>6.清空数据区域事件</h3>

```csharp
/// <summary>
/// 清空接收区
/// </summary>
/// <param name="sender"></param>
/// <param name="e"></param>
private void btnClearRev_Click(object sender, EventArgs e)
{
    txtShowData.Clear();
}

/// <summary>
/// 清空发送区
/// </summary>
/// <param name="sender"></param>
/// <param name="e"></param>
private void btnClearSend_Click(object sender, EventArgs e)
{
    txtSendData.Clear();
}

```

<p><a href="http://files.cnblogs.com/files/JiYF/BXHSerialPort.zip" target="_blank"><span style="color: #339966;"><span style="color: #339966;">&nbsp;运行程序下载地址</span>&nbsp;&nbsp;BXHSerialPort.exe</span></a></p>
<p><a href="http://download.csdn.net/detail/coderjyf/9826709" target="_blank">源代码工程文件下载</a></p>

OK 步入正题
<h1>二、Unity3d与串口通信程序的开发</h1>

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMjA0MTU0NDIxMTAx?x-oss-process=image/format,png)
先做一个简单的UI，用来控制串口设备的开关


代码就比较简单了
<h2>自定义端口API类</h2>

```csharp
using System;
//先要引入这个命名空间
using System.IO.Ports;

//这个是连接上的串口设备的定义好的参数，发送这个参数就能控制串口设备
public enum PortsType
{
   //01、全开：PC发送'I'； 
   //02、全关：PC发送'i'; 
   I,i,
   //03、第一路开：PC发送'A'；
   //04、第二路开：PC发送'B'；
   //05、第三路开：PC发送'C'；
   //06、第四路开：PC发送'D'；
   //07、第五路开：PC发送'E'；
   //08、第六路开：PC发送'F'；
   //09、第七路开：PC发送'G'；
   //10、第八路开：PC发送'H'；
   A,B,C,D,E,F,G,H,
   //11、第一路关：PC发送'a'；
   //12、第二路关：PC发送'b'；
   //13、第三路关：PC发送'c'；
   //14、第四路关：PC发送'd'；
   //15、第五路关：PC发送'e'；
   //16、第六路关：PC发送'f'；
   //17、第七路关：PC发送'g'；
   //18、第八路关：PC发送'h'；
   a,b,c,d,e,f,g,h
}
public class PortsContol
{
	//第一个参数是端口的名字，一会说怎么看端口，第二个参数是波特率，这个是设备自身的参数
    SerialPort sp = new SerialPort("COM3", 9600);//声明一个串口类
    
    //这个是完整的参数，名别是 端口名、波特率、奇偶效验、数据位、流控制参数
    //SerialPort sp1 = new SerialPort("COM3", 9600, Parity.None, 8, StopBits.None);
    
    //这个就是一个发送的API，其他程序就调用这个API
    public void Send_Click(PortsType data)
    {
        sp.Open();
        switch (data)
        {
            case PortsType.I:
            //给端口发送数据
                sp.WriteLine("I");
                break;
            case PortsType.i:
                sp.WriteLine("i");
                break;
            case PortsType.A:
                sp.WriteLine("A");
                break;
            case PortsType.B:
                sp.WriteLine("B");
                break;
            case PortsType.C:
                sp.WriteLine("C");
                break;
            case PortsType.D:
                sp.WriteLine("D");
                break;
            case PortsType.E:
                sp.WriteLine("E");
                break;
            case PortsType.F:
                sp.WriteLine("F");
                break;
            case PortsType.G:
                sp.WriteLine("G");
                break;
            case PortsType.H:
                sp.WriteLine("H");
                break;
            case PortsType.a:
                sp.WriteLine("a");
                break;
            case PortsType.b:
                sp.WriteLine("b");
                break;
            case PortsType.c:
                sp.WriteLine("c");
                break;
            case PortsType.d:
                sp.WriteLine("d");
                break;
            case PortsType.e:
                sp.WriteLine("e");
                break;
            case PortsType.f:
                sp.WriteLine("f");
                break;
            case PortsType.g:
                sp.WriteLine("g");
                break;
            case PortsType.h:
                sp.WriteLine("h");
                break;
            default:
                break;
        }
        sp.Close();
    }
}

```
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMjA0MTU1NzQwODA1?x-oss-process=image/format,png)

![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMjA0MTU1ODE2MTQy?x-oss-process=image/format,png)

<h2>Unity调用</h2>

```csharp
using UnityEngine;
using System.Collections;

public class ButtonClick1 : MonoBehaviour
{
	//初始化自定义端口类
    PortsContol pc = new PortsContol();

    // Use this for initialization
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {

    }
    //按钮"开"调用的函数
    public void Open_Click()
    {
	    //参数是自定义类中的枚举
        pc.Send_Click(PortsType.I);
    }
    //按钮"关"调用的函数
    public void Close_Click()
    {
        pc.Send_Click(PortsType.i);
    }
    //按钮"1开"调用的函数
    public void OneOpen_Click()
    {
        pc.Send_Click(PortsType.A);
    }
    //按钮"1关"调用的函数
    public void OneClose_Click()
    {
        pc.Send_Click(PortsType.a);
    }
}

```
脚本绑定在主摄像机上，按钮Button调用主摄像机上的函数
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMjA0MTYwNDAzNTQw?x-oss-process=image/format,png)

OK，这就行了，就可以用Unity3d控制串口程序了


</body>
</html>
