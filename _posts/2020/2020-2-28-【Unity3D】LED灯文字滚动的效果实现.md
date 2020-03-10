---
layout: post
category: Unity3D-Daily
title: 【Unity3D】LED灯文字滚动的效果实现
tagline: by 恬静的小魔龙
tag: Unity3D
---

大体工作原理是这样的，首先，计算出你整个屏能显示几行文字，按照文字的行数把大屏等分为n份，也就是将黑色的屏幕部分做成宽度和文字横高相等的片状模型，个数同整屏显示文字的行数相同。然后，将要显示的文字拿相机全部拍下来，按顺序做成一整张图，然后将整张图按行切分，有几行文字切分几行，记住还要做一个没有文字的空白行，显示无文字状态。接下来就交给代码来控制了，比方说led屏能显示9行文字那么你就做10行片状模型，最后一行又led外边框遮盖，在正面显示不出来的。然后，给10个模型赋贴图，并让10个模型一起向上均匀移动，等到恰好移动一行贴图的高度的时候，将10片模型归为原位，然后，从新赋n-1行内容的贴图，以此类推，等到所有文字全部走完，恢复初始状态继续循环。


<img src="http://img.blog.csdn.net/20171028171632485?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcTc2NDQyNDU2Nw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast"></img>

/   led.js 将这个js文件托给pz模型这个led就可以运行了。
private var p0:GameObject;
private var p1:GameObject;
private var p2:GameObject;
private var p3:GameObject;
private var p4:GameObject;
private var p5:GameObject;
private var p6:GameObject;
private var p7:GameObject;
private var p8:GameObject;
private var p9:GameObject;
private var pz:GameObject;
private var time:float;
private var flag:float=2.29966f;  //间隔多长时间更换一次贴图
private var nowFram:int;
private var Value_h:float=0.0f;
private var n:int=10;        //移动速度调节参数           这两个参数你可以根据你显示的效果自行调节

private var pos1:Vector3;
private var pos2:Vector3;
private var pos3:Vector3;
private var pos4:Vector3;
private var pos5:Vector3;
private var pos6:Vector3;
private var pos7:Vector3;
private var pos8:Vector3;
private var pos9:Vector3;
public var textSingle:Texture[];  //贴图赋值

function Start () {
p1=GameObject.Find("p1");
p2=GameObject.Find("p2");
p3=GameObject.Find("p3");
p4=GameObject.Find("p4");
p5=GameObject.Find("p5");
p6=GameObject.Find("p6");
p7=GameObject.Find("p7");
p8=GameObject.Find("p8");
p9=GameObject.Find("p9");
pz=GameObject.Find("pz");
      
 Value_h=p1.GetComponent(MeshFilter).mesh.bounds.size.x/n * pz.transform.localScale.x;  //文字的移动速度不随模型的放缩而改变
  pos1=p1.transform.position;
    pos2=p2.transform.position;
      pos3=p3.transform.position;
        pos4=p4.transform.position;
          pos5=p5.transform.position;
            pos6=p6.transform.position;
              pos7=p7.transform.position;
                pos8=p8.transform.position;
                  pos9=p9.transform.position;                 


Initial();//初始化

}


function FixedUpdate(){
 
p1.transform.Translate(-Vector3.right * Value_h);
p2.transform.Translate(-Vector3.right * Value_h);
p3.transform.Translate(-Vector3.right * Value_h);
p4.transform.Translate(-Vector3.right * Value_h);
p5.transform.Translate(-Vector3.right * Value_h);
p6.transform.Translate(-Vector3.right * Value_h);
p7.transform.Translate(-Vector3.right * Value_h);
p8.transform.Translate(-Vector3.right * Value_h);
p9.transform.Translate(-Vector3.right * Value_h);
time+=Time.deltaTime;
if(time>=flag){
p1.transform.position=pos1;
p2.transform.position=pos2;
p3.transform.position=pos3;
p4.transform.position=pos4;
p5.transform.position=pos5;
p6.transform.position=pos6;
p7.transform.position=pos7;
p8.transform.position=pos8;
p9.transform.position=pos9;
DrawAnimation(textSingle);
 }
 
}

function DrawAnimation(tex : Texture[])
{ 
   
   nowFram++;
   time=0;
   if(nowFram>=25){
     nowFram=0;
   
   }
  
   p1.renderer.material.mainTexture=tex[nowFram];
   p2.renderer.material.mainTexture=tex[nowFram+1];
    p3.renderer.material.mainTexture=tex[nowFram+2];
     p4.renderer.material.mainTexture=tex[nowFram+3];
      p5.renderer.material.mainTexture=tex[nowFram+4];
       p6.renderer.material.mainTexture=tex[nowFram+5];
        p7.renderer.material.mainTexture=tex[nowFram+6];
         p8.renderer.material.mainTexture=tex[nowFram+7];
          p9.renderer.material.mainTexture=tex[nowFram+8];
          
}


function Initial()
{


 p1.renderer.material.mainTexture=textSingle[0];
 p2.renderer.material.mainTexture=textSingle[1];
 p3.renderer.material.mainTexture=textSingle[2];
 p4.renderer.material.mainTexture=textSingle[3];
 p5.renderer.material.mainTexture=textSingle[4];
 p6.renderer.material.mainTexture=textSingle[5];
 p7.renderer.material.mainTexture=textSingle[6];
 p8.renderer.material.mainTexture=textSingle[7];
 p9.renderer.material.mainTexture=textSingle[8];


}
