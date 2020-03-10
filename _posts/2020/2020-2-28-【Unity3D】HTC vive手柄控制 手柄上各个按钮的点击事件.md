---
layout: post
category: Unity3D-Daily
title: 【Unity3D】HTC vive手柄控制 手柄上各个按钮的点击事件
tagline: by 恬静的小魔龙
tag: Unity3D
---

HTC vive手柄各个按钮的响应事件，已实现的功能是按手柄上的原盘上下左右键移动，是平移，不是射线瞬移。这种方式在眼镜里太晕了。

using UnityEngine;  
using System.Collections;  
  
public class Move : MonoBehaviour {  
  
    /// <summary>    
    /// 手柄位置    
    /// </summary>    
    SteamVR_TrackedObject tracked;  
  
  
    /// <summary>    
    /// 玩家    
    /// </summary>    
    public Transform player;  
  
    /// <summary>    
    /// 方向     
    /// </summary>    
    public Transform dic;  
  
    /// <summary>    
    /// 速度    
    /// </summary>    
    public float speed;  
  
    void Awake()  
    {  
        //获取手柄控制    
        tracked = GetComponent<SteamVR_TrackedObject>();  
  
    }  
  
    // Use this for initialization    
    void Start()  
    {  
  
    }  
  
    // Update is called once per frame    
    void FixedUpdate()  
    {  
        var deviceright = SteamVR_Controller.Input((int)tracked.index);  
  
  
        //按下圆盘键    
        if (deviceright.GetPress(SteamVR_Controller.ButtonMask.Touchpad))  
        {  
  
            Vector2 cc = deviceright.GetAxis();  
  
            float angle = VectorAngle(new Vector2(1, 0), cc);  
  
            //下    
            if (angle > 45 && angle < 135)  
            {  
  
                player.Translate(-dic.forward * Time.deltaTime * speed);  
            }  
            //上      
            else if (angle < -45 && angle > -135)  
            {  
                //Debug.Log("上");    
                player.Translate(dic.forward * Time.deltaTime * speed);  
            }  
            //左      
            else if ((angle < 180 && angle > 135) || (angle < -135 && angle > -180))  
            {  
                //Debug.Log("左");    
                player.Translate(-dic.right * Time.deltaTime * speed);  
            }  
            //右      
            else if ((angle > 0 && angle < 45) || (angle > -45 && angle < 0))  
            {  
                //Debug.Log("右");    
                player.Translate(dic.right * Time.deltaTime * speed);  
            }  
        }  
        else if (deviceright.GetPressDown(SteamVR_Controller.ButtonMask.Trigger))  
        {  
  
            Debug.Log("按下扳机键");  
        }  
        else if (deviceright.GetPressDown(SteamVR_Controller.ButtonMask.Grip))  
        {  
  
            Debug.Log("按下手柄侧键");  
        }  
        else if (deviceright.GetPressDown(SteamVR_Controller.ButtonMask.ApplicationMenu))  
        {  
  
            Debug.Log("按下手柄菜单键");  
        }  
        else if (deviceright.GetPressDown(SteamVR_Controller.ButtonMask.ApplicationMenu))  
        {  
  
            Debug.Log("按下手柄菜单键");  
        }  
    }  
  
  
  
    /// <summary>    
    /// 根据在圆盘才按下的位置，返回一个角度值    
    /// </summary>    
    /// <param name="from"></param>    
    /// <param name="to"></param>    
    /// <returns></returns>    
    float VectorAngle(Vector2 from, Vector2 to)  
    {  
        float angle;  
        Vector3 cross = Vector3.Cross(from, to);  
        angle = Vector2.Angle(from, to);  
        return cross.z > 0 ? -angle : angle;  
    }  
}  