---
layout: post
category: Unity3D-Daily
title: 【Unity3D】Unity 几种画线方式
tagline: by 恬静的小魔龙
tag: Unity3D
---

##【前言】
图形化调试可以加速开发。
例如在战斗中，可能需要知道所有单位的仇恨值，如果这些信息全打log的话，很难有直观感受，
而如果在Scene窗口里，单位头顶有一个球，越红表示仇恨越高，越暗表示仇恨越低，那么调试起来比打log直观多了。
 
##【一 图形化调试】
Unity中图形化调试主要4种
####Debug.Draw
####Gizmos.Draw
####Graphic.DrawMesh
####GL
只需在Scene窗口显示的调试图像
     一直显示的 OnDrawGizmos + Gizmos.Draw
     选中显示的 OnDrawGizmosSelected + Gizmos.Draw
     脚本控制的 Update + Debug.Draw
需要在实际设备屏幕显示的调试图像
    Update+Graphic.DrawMesh
    OnRenderObject+GL
 
Graphic.DrawMesh和Debug.Draw   调用一致，都是在Update系里
Graphic.DrawMesh和GL       显示类似，都在各个窗口显示，并且可以设置材质。
四种方式比较
####（1）Debug.Draw
=1=一般在Update/Fixed Update/LateUpdate里调用
=2=只在Scene窗口里显示
=3=并且不能设置材质
 

```
    void Update()
    {
        Debug.DrawLine (worldPos1, worldPos2,Color.yellow);
    }
```

 
####（2）Gizmos.Draw
=1=在OnDrawGizmos /OnDrawGizmosSelected里调用
=2=只在Scene窗口里显示
=3=并且不能设置材质
```
    public void OnDrawGizmosSelected() {
        Gizmos.DrawLine(Vector3.zero, new Vector3(0,3f,0));
    }
```
####（3）Graphic.DrawMesh
=1=一般在Update/Fixed Update/LateUpdate里调用
=2=实际屏幕和Scene窗口都能显示
=3=可以设置材质
画Mesh Ok
```
         void Update()
        {
            Graphics.DrawMesh(mesh, worldPos, worldRotation, material, 0);
        }
```
####（4）GL,
=1=一般在物体的OnRenderObject 或者相机的OnPostRender里调用
=2=实际屏幕和Scene窗口都能显示
=3=可以设置材质
一个GL.Begin/GL.End里的渲染是自动合并的，一般是一个Drawcall
画一些线，三角可以。用GL.TRIANGLES 显示整个Mesh的话会超卡。

例：渲染线框
```
      void OnRenderObject()
    {
        mat.SetPass(0);
         GL.wireframe = true;
         GL.Color (new Color (1,1, 0, 0.8F));
        GL.PushMatrix();
        GL.Begin(GL.TRIANGLES);
        for(int i=0;i<</span>mesh.triangles.Length-2;i+=3)
        {
            GL.Vertex(mesh.vertices[mesh.triangles[i]]);
            GL.Vertex(mesh.vertices[mesh.triangles[i+1]]);
            GL.Vertex(mesh.vertices[mesh.triangles[i+2]]);
        }
        GL.End();
        GL.PopMatrix();
              GL.wireframe = false;
    }
```
 
####【二 GL】
GL除了可以用来调试，可以拿来做功能，例如LineRenderer，地格等。
 
GL即Graphics Library。Low-Level Graphics Library。计算matrices,发出类似OpenGL的immediate模式的渲染指令，和其他低级图像任务。Graphic.DrawMesh()比GL更高效。
GL立即绘制函数只用当前material的设置。因此除非你显示指定mat,否则mat可以是任何材质。并且GL可能会改变材质。
GL是立即执行的，如果你在Update（）里调用，它们将在相机渲染前执行，相机渲染将会清空屏幕，GL效果将无法看到。
通常GL用法是
在camera上贴脚本，并在OnPostRender()里执行。
也可以挂在任何GameObject上，在OnRenderObject()里执行。
或者挂在物体上
注意: 
1.GL的线等基本图元并没有uv. 所有是没有贴图纹理影射的，shader里仅仅做的是单色计算或者对之前的影像加以处理。
2.GL所使用的shader里必须有Cull off指令，否则显示会变成如下
3. 如果是线，颜色是GL.Color( new Color(1,1,1,0.5f) );设置的颜色
   如果是GL.TRIANGLES或者是GL.QUADS,则颜色是shader里的颜色。
1.
GL.PushMatrix()
保存matrices至matrix stack上。
GL.PopMatrix()
从matrix stack上读取matrices。
2.
GL.LoadPixelMatrix()
改变MVP矩阵，使得transform里的xy 直接对应像素，（0，0）表示屏幕viewport的左下角，z的范围是(-1,1),该函数改变camera的参数，所以需要GL.PushMatrix()保存和GL.PopMatrix()读取。
GL.Vertex3（）的取值范围从左下角的(0,0,0) 至右上角的（Screen.width,Screen.height,0）
GL.LoadOrtho()
设置ortho perspective，即水平视角。After calling LoadOrtho, the viewing frustum goes from (0,0,-1) to (1,1,100). 主要用于在纯2D里绘制图元。
GL.Vertex3（）的取值范围从左下角的(0,0,0) 至右上角的（1,1,0）
3.
OnPostRender()
只有物体上有激活的摄像机时，才会调用的函数，当摄像机完成渲染场景，绘制了所有物体以后调用。
OnPostRender可以变成co-routine,加yield语句即可。

WaitForEndOfFrame()
等待至 所有绘制之后，end of frame, 就在展示frame到屏幕之前。可以做截图。可以在任何物体上使用该函数。

###例1:屏幕画线  
```
using UnityEngine;  
using System.Collections;  
  
public class GLTest : MonoBehaviour {  
  
  public Material mat;  
    void OnPostRender() {  
        if (!mat) {  
            Debug.LogError("Please Assign a material on the inspector");  
            return;  
        }  
        GL.PushMatrix(); //保存当前Matirx  
        mat.SetPass(0); //刷新当前材质  
        GL.LoadPixelMatrix();//设置pixelMatrix  
        GL.Color(Color.yellow);  
        GL.Begin(GL.LINES);  
        GL.Vertex3(0, 0, 0);  
        GL.Vertex3(Screen.width, Screen.height, 0);  
        GL.End();  
        GL.PopMatrix();//读取之前的Matrix  
    }  
}  
```

###例2：截图  
   
```
using System.IO;  
using UnityEngine;  
using System.Collections;  
  
public class ScreenShot : MonoBehaviour {  
    void Start() {  
        StartCoroutine(UploadPNG() );  
    }  
    IEnumerator UploadPNG() {  
        yield return new WaitForEndOfFrame();  
print ("yuuuuu");  
        int width = Screen.width;  
        int height = Screen.height;  
        Texture2D tex = new Texture2D(width, height, TextureFormat.RGB24, false);  
        tex.ReadPixels(new Rect(0, 0, width, height), 0, 0);  
        tex.Apply();  
        byte[] bytes = tex.EncodeToPNG();  
File.WriteAllBytes(Application.dataPath+"/ss.png",bytes);  
UnityEditor.AssetDatabase.Refresh();  
    }  
}  
```


例3：展示Alpha  
  
```
using UnityEngine;  
using System.Collections;  
  
public class GLTest : MonoBehaviour {  
public Shader shader;  
public Texture2D t2d;  
  private Material mat;  
void Start()  
{  
mat = new Material(shader);  
mat.mainTexture = t2d;  
}  
    void OnPostRender() {  
        if (!mat) {  
            Debug.LogError("Please Assign a material on the inspector");  
            return;  
        }  
        GL.PushMatrix();  
        mat.SetPass(0);  
        GL.LoadOrtho();  
        GL.Begin(GL.QUADS);  
        GL.Vertex3(0, 0, 0.1F);  
        GL.Vertex3(1f, 0, 0.1F);  
        GL.Vertex3(1f, 1, 0.1F);  
        GL.Vertex3(0, 1, 0.1F);  
        GL.End();  
        GL.PopMatrix();  
    }  
}  
Shader "Custom/GLDrawLine" {  
Properties {  
_MainTex ("Base (RGB)", 2D) = "white" {}  
}  
SubShader {  
    Pass {  
Cull off  
Blend DstAlpha zero  
Color(1,1,1,1)  
    }  
}  
}  
```
