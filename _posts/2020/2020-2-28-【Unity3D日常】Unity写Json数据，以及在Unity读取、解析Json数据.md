---
layout: post
category: Unity3D-Daily
title: 【Unity3D日常】Unity写Json数据，以及在Unity读取、解析Json数据
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
在日常开发中，会遇到要保存数据，或者发送数据的需求，数据保存通常用Json或者XML，今天我们就来看一下如何在Unity中读写Json数据，以及解析Json数据，包括多段数据的解析


## 二、单条Json数据的读写、解析
### 1.单条Json数据的写入
首先，我们先写一个字段类Person，类里面有string类型的“Name”和int类型的“Grade”，然后写一个"Data”数据类，里面存放的使我们的字段类Person：
```csharp
[System.Serializable]
class Person
{
    public string Name;
    public int Grade;
}
[System.Serializable]
class Data
{
    public Person Person;
}
```
首先是写Json数据：

```csharp
	//写数据
    public void WriteData()
    {
    	//新建一个字段类 进行赋值
        Person m_Person = new Person();
        m_Person.Name = "User1";
        m_Person.Grade = 13;
        //新建一个数据类 将字段类赋值
        Data m_Data = new Data();
        m_Data.Person = m_Person;
        //将数据转成json
        string js = JsonUtility.ToJson(m_Data);
        //获取到项目路径
        string fileUrl = Application.streamingAssetsPath + "\\jsonInfo2.txt";
        //打开或者新建文档
        StreamWriter sw = new StreamWriter(fileUrl);
        //保存数据
        sw.WriteLine(js);
        //关闭文档
        sw.Close();
    }
```
对啦，你要新建一个StreamingAssets文件夹，不然会报错：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190926154847220.png)
数据如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190926154906704.png)
用网站解析也没有问题：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190926154942202.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
### 2.单条Json数据的读取
读取就很简单：

```csharp
	//读取文件
    public string ReadData()
    {
    	//string类型的数据常量
        string readData = "";
        //获取到路径
        string fileUrl = Application.streamingAssetsPath+ "\\jsonInfo2.txt";
        //读取文件
        StreamReader str = File.OpenText(fileUrl);
        //数据保存
        readData = str.ReadToEnd();
        str.Close();
        //返回数据
        return readData;
    }
```
来，让我们来获取一下数据看对不对：

```csharp
	void Start()
    {
        string jsonData = ReadData();
        Debug.Log(jsonData);
    }
```
看控制台输出：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190926155510441.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
### 3.单条Json数据的解析
```csharp
	void Start()
    {
        string jsonData = ReadData();
        Debug.Log(jsonData);
        //数据解析并把数据保存到m_PersonData变量中
		Data m_PersonData = JsonUtility.FromJson<Data>(jsonData);
		//读取数据
        Debug.Log(m_PersonData.Person.Name);
        Debug.Log(m_PersonData.Person.Grade);
    }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190926155716530.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
完整代码如下：

```csharp
using System.IO;
using UnityEngine;

[System.Serializable]
class Person
{
    public string Name;
    public int Grade;
}
[System.Serializable]
class Data
{
    public Person Person;
}

public class TestToJson : MonoBehaviour
{
    void Start()
    {
        //WriteData();
        string jsonData = ReadData();
        Debug.Log(jsonData);

        Data m_PersonData = JsonUtility.FromJson<Data>(jsonData);
        Debug.Log(m_PersonData.Person.Name);
        Debug.Log(m_PersonData.Person.Grade);
    }


    //读取文件
    public string ReadData()
    {
        string readData = "";
        string fileUrl = Application.streamingAssetsPath+ "\\jsonInfo2.txt";
        StreamReader str = File.OpenText(fileUrl);
        readData = str.ReadToEnd();
        str.Close();
        return readData;
    }

    //写数据
    public void WriteData()
    {
        Person m_Person = new Person();
        m_Person.Name = "User1";
        m_Person.Grade = 13;
        Data m_Data = new Data();
        m_Data.Person = m_Person;
        string js = JsonUtility.ToJson(m_Data);
        string fileUrl = Application.streamingAssetsPath + "\\jsonInfo2.txt";
        StreamWriter sw = new StreamWriter(fileUrl);
        sw.WriteLine(js);
        sw.Close();
    }
}

```
## 三、多条Json数据的读写、解析
### 1.多条Json数据的写入
首先，我们先写一个字段类Person，类里面有string类型的“Name”和int类型的“Grade”，然后写一个"Data1”数据类，里面存放的使我们的字段类Person数组：
```csharp
[System.Serializable]
class Person
{
    public string Name;
    public int Grade;
}
[System.Serializable]
class Data1
{
    public Person[] Person;
}
```
首先是写Json数据：

```csharp
	//写数据
    public void WriteData()
    {
        //新建一个数据类
        Data1 m_Data = new Data1();
        //新建一个字段类 进行赋值
        m_Data.Persons = new Person[5];
        for (int i = 0; i < 5; i++)
        {
            Person m_Person = new Person();
            m_Person.Name = "User" + i;
            m_Person.Grade = i + 50;
            m_Data.Persons[i] = m_Person;
        }
        //将数据转成json
        string js = JsonUtility.ToJson(m_Data);
        //获取到项目路径
        string fileUrl = Application.streamingAssetsPath + "\\jsonInfo3.txt";
        //打开或者新建文档
        StreamWriter sw = new StreamWriter(fileUrl);
        //保存数据
        sw.WriteLine(js);
        //关闭文档
        sw.Close();
    }
```
对啦，你要新建一个StreamingAssets文件夹，不然会报错：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190926154847220.png)
数据如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190926160310247.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
用网站解析也没有问题：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190926160324392.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
### 2.多条Json数据的读取
读取就很简单：

```csharp
	//读取文件
    public string ReadData()
    {
    	//string类型的数据常量
        string readData = "";
        //获取到路径
        string fileUrl = Application.streamingAssetsPath+ "\\jsonInfo3.txt";
        //读取文件
        StreamReader str = File.OpenText(fileUrl);
        //数据保存
        readData = str.ReadToEnd();
        str.Close();
        //返回数据
        return readData;
    }
```
来，让我们来获取一下数据看对不对：

```csharp
	void Start()
    {
        string jsonData = ReadData();
        Debug.Log(jsonData);
    }
```
看控制台输出：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190926160432823.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
### 3.多条Json数据的解析
```csharp
	void Start()
    {
        string jsonData = ReadData();
        Debug.Log(jsonData);
        //数据解析并把数据保存到m_PersonData1 变量中
        Data1 m_PersonData1 = JsonUtility.FromJson<Data1>(jsonData);
        foreach (var item in m_PersonData1.Persons)
        {
            Debug.Log(item.Name);
            Debug.Log(item.Grade);
        }
    }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190926160705342.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
完整代码如下：

```csharp
using System.IO;
using UnityEngine;

[System.Serializable]
class Person
{
    public string Name;
    public int Grade;
}
[System.Serializable]
class Data
{
    public Person Person;
}
[System.Serializable]
class Data1
{
    public Person[] Persons;
}

public class TestToJson : MonoBehaviour
{
    void Start()
    {
        //WriteData2();
        //WriteData3();
        
		//单条数据
        //string jsonData = ReadData();
        //Debug.Log(jsonData);
        //Data m_PersonData = JsonUtility.FromJson<Data>(jsonData);
        //Debug.Log(m_PersonData.Person.Name);
        //Debug.Log(m_PersonData.Person.Grade);

		//多条数据
        string jsonData = ReadData();
        Debug.Log(jsonData);
        Data1 m_PersonData1 = JsonUtility.FromJson<Data1>(jsonData);
        foreach (var item in m_PersonData1.Persons)
        {
            Debug.Log(item.Name);
            Debug.Log(item.Grade);
        }
    }


    //读取文件
    public string ReadData()
    {
        string readData = "";
        string fileUrl = Application.streamingAssetsPath+ "\\jsonInfo3.txt";
        StreamReader str = File.OpenText(fileUrl);
        readData = str.ReadToEnd();
        str.Close();
        return readData;
    }

    //写数据
    public void WriteData2()
    {
        Person m_Person = new Person();
        m_Person.Name = "User1";
        m_Person.Grade = 13;
        Data m_Data = new Data();
        m_Data.Person = m_Person;
        string js = JsonUtility.ToJson(m_Data);
        string fileUrl = Application.streamingAssetsPath + "\\jsonInfo2.txt";
        StreamWriter sw = new StreamWriter(fileUrl);
        sw.WriteLine(js);
        sw.Close();
    }

    //写数据
    public void WriteData3()
    {
        Data1 m_Data = new Data1();
        m_Data.Persons = new Person[5];
        for (int i = 0; i < 5; i++)
        {
            Person m_Person = new Person();
            m_Person.Name = "User" + i;
            m_Person.Grade = i + 50;
            m_Data.Persons[i] = m_Person;
        }
        string js = JsonUtility.ToJson(m_Data);
        string fileUrl = Application.streamingAssetsPath + "\\jsonInfo3.txt";
        StreamWriter sw = new StreamWriter(fileUrl);
        sw.WriteLine(js);
        sw.Close();
    }
}
```

总结：
1.不要自己写json串，不然一个中括号或者花括号写错就死活解不出来
2.需要先明确自己接收的json串的格式，然后再写自己的数据类
3.如果不牵扯到服务器的数据，都是本地数据的存写的话，最好存写都自己来写
