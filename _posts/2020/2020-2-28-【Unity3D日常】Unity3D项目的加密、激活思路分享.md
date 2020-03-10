---
layout: post
category: Unity3D-Daily
title: 【Unity3D日常】Unity3D项目的加密、激活思路分享
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
今天分享几种比较简单的项目加密、激活的思路，但是不是对项目的源代码加密，只是对不是太懂破解的人有用的几种加密思路，当然，万物能加密就能解密，只是时间问题 

## 二、正文

### 思路一：项目中保存账号密码

这个可以使用常量用来保存账号密码，或者注册表，或者本地持久化数据，思路就是先将账号密码设置好，然后其他人用这个软件的时候需要账号密码来激活，用以加密

过程：
1 我们先做一个UI
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190920151458518.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
2 代码

```csharp
using UnityEngine;
using UnityEngine.UI;

public class Encript2 : MonoBehaviour
{
    //设置一个初始的账号和密码 
    public string UserName = "";
    public string PassWord = "";
    //UI
    public Text m_UserNameText;
    public Text m_PassWordText;
    public Text m_HelpText;
    public Button m_BtnAticvation;
    public Button m_BtnExit;
    
    void Start()
    {
        m_BtnAticvation.onClick.AddListener(BtnEvent_Aticvation);
        m_BtnExit.onClick.AddListener(BtnEvent_Exit);
        //判断是否已经激活
        Decide_Acticvation();
    }

    public void Decide_Acticvation()
    {
        if (UserName == "admin" && PassWord == "123456")
        {
            gameObject.SetActive(false);
        }
    }

    public void BtnEvent_Aticvation()
    {
        UserName = m_UserNameText.text;
        PassWord = m_PassWordText.text;
        if (m_UserNameText.text == "admin" && m_PassWordText.text == "123456")
        {
            //激活成功
            gameObject.SetActive(false);
        }
        else
        {
            //未激活成功
            m_HelpText.text = "账号密码不正确";
        }
    }

    public void BtnEvent_Exit()
    {
        Application.Quit();
    }
}
```
3 绑定参数
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190920151538823.png)
整体就思路很简单，设置一个本地的常量，然后进入软件需要先激活，激活成功

优点：简单粗暴
缺点：容易被破解


### 思路二：在本地写入保存的有加密数据的文件

这个呢，其实也很简单，就在首先在客户端写一个验证程序，验证 当前电脑的设备唯一标识加自定义字段，之后加密后的文件数据
 对比，是否相同，相同就进入程序，不同就关闭程序 
然后在电脑端写一个激活程序

过程：
1、首先做一个激活的界面
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019092111312490.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190921113131556.png)
做的比较丑，最重要的是功能！！！
2、代码

```csharp
using Microsoft.Win32;
using System.Security.Cryptography;
using System.Text;
using UnityEngine;

public class Encript3 : MonoBehaviour
{
    public GameObject m_PanelEncrypted;

    void Start()
    {
        string s1 = SystemInfo.deviceUniqueIdentifier;
        string s2 = GetMD5String(s1);
        //验证本地的加密字符串是否正确
        Verification_MD5(s2);
    }

    //验证本地的md5正确与否
    public void Verification_MD5(string ValidateMD5)
    {
        RegistryKey RootKey, RegKey;
        RootKey = Registry.CurrentUser.OpenSubKey("UserSetting", true);
        //打开子项：HKEY_CURRENT_USER\UserSetting\Encrypted
        if ((RegKey = RootKey.OpenSubKey("Encrypted", true)) == null)//如果文件不存在，就显示要激活的界面
        {
            m_PanelEncrypted.SetActive(true);
        }
        //异常捕捉，如果出现程序异常，比如闪退
        try
        {
            object encrypted = RegKey.GetValue("EncryptedString");        //读取键值
            if (encrypted.ToString() == ValidateMD5)
            {
                m_PanelEncrypted.SetActive(false);
            }
            else
            {
                m_PanelEncrypted.SetActive(true);
            }
        }
        catch
        {
            print("异常状况");
        }
    }

    /// <summary>
    /// 通过字符串获取MD5值，返回32位字符串。
    /// </summary>
    /// <param name="str"></param>
    /// <returns></returns>
    public static string GetMD5String(string text)
    {
        MD5CryptoServiceProvider md5Hasher = new MD5CryptoServiceProvider();
        byte[] data = md5Hasher.ComputeHash(Encoding.Default.GetBytes(text));
        StringBuilder sBuilder = new StringBuilder();
        for (int i = 0; i < data.Length; i++)
        {
            sBuilder.Append(data[i].ToString("x2"));
        }
        return sBuilder.ToString().ToLower();
    }
}
```

OK，小伙伴们可以试试了。。对啦，还有激活程序。。
代码：

```csharp
/// <summary>
    /// 通过字符串获取MD5值，返回32位字符串。
    /// </summary>
    /// <param name="str"></param>
    /// <returns></returns>
    public static string GetMD5String(string text)
    {
        MD5CryptoServiceProvider md5Hasher = new MD5CryptoServiceProvider();
        byte[] data = md5Hasher.ComputeHash(Encoding.Default.GetBytes(text));
        StringBuilder sBuilder = new StringBuilder();
        for (int i = 0; i < data.Length; i++)
        {
            sBuilder.Append(data[i].ToString("x2"));
        }
        return sBuilder.ToString().ToLower();
    }

    //激活程序
    public void Activation()
    {
        string md5 = GetMD5String(SystemInfo.deviceUniqueIdentifier);
        // 创建键值对
        RegistryKey RootKey, RegKeyRoot,RegKey;
        //项名为：HKEY_CURRENT_USER
        RootKey = Registry.CurrentUser;
        if ((RegKeyRoot = RootKey.OpenSubKey("UserSetting",true))== null)
        {
            RootKey.CreateSubKey("UserSetting");
            RegKeyRoot = RootKey.OpenSubKey("UserSetting", true);
        }
        //打开子项：HKEY_CURRENT_USER\Software\TestToControlUseTime
        if ((RegKey = RegKeyRoot.OpenSubKey("Encrypted", true)) == null)
        {
            RegKeyRoot.CreateSubKey("Encrypted");               //不存在，则创建子项
            RegKey = RootKey.OpenSubKey("Encrypted", true);  //打开键值
            RegKey.SetValue("EncryptedString", (object)md5);         //创建键值
            return;
        }
        else
        {
            RegKey.SetValue("EncryptedString", (object)md5);
            return;
        }
    }
```
总结：可以自己写加密程序，或者加一些其他的参数，增加破解的难度

优点：难破解，安全性更高
缺点：还是不够安全，如果破解源代码的话，就知道怎么加密了，就可以反向解密

### 思路三：在服务器端加密，返回加密的字符串
这个思路就是将加密程序放到服务器，其他人就算是破解了代码，也不知道加密思路，就无法反向解密

过程：
1、界面设计
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019092111312490.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190921113131556.png)
做的比较丑，最重要的是功能！！！

2、代码

```csharp
using Microsoft.Win32;
using System.Collections;
using UnityEngine;

public class Encript4 : MonoBehaviour
{
    public GameObject m_PanelEncrypted;
    string md5 = "";

    void Start()
    {
        StartCoroutine(GetData());
        //验证本地的加密字符串是否正确
        Verification_MD5(md5);
    }

    //验证本地的md5正确与否
    public void Verification_MD5(string ValidateMD5)
    {
        RegistryKey RootKey, RegKey;
        RootKey = Registry.CurrentUser.OpenSubKey("UserSetting", true);
        //打开子项：HKEY_CURRENT_USER\UserSetting\Encrypted
        if ((RegKey = RootKey.OpenSubKey("Encrypted", true)) == null)//如果文件不存在，就显示要激活的界面
        {
            m_PanelEncrypted.SetActive(true);
        }
        //异常捕捉，如果出现程序异常，比如闪退
        try
        {
            object encrypted = RegKey.GetValue("EncryptedString");        //读取键值
            if (encrypted.ToString() == ValidateMD5)
            {
                m_PanelEncrypted.SetActive(false);
            }
            else
            {
                m_PanelEncrypted.SetActive(true);
            }
        }
        catch
        {
            print("异常状况");
        }
    }

    public IEnumerator GetData()
    {
        WWW www = new WWW("http://127.0.0.1/Test.php?md5="+SystemInfo.deviceUniqueIdentifier);
        yield return www;
        if (www.error != null)
        {
            yield return null;
        }
        else
        {
            md5 = www.text;
        }
    }
}
```


OK，今天就到这吧
