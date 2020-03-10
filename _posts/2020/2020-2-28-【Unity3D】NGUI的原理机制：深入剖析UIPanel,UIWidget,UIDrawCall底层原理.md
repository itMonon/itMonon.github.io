---
layout: post
category: Unity3D-Daily
title: 【Unity3D】NGUI的原理机制：深入剖析UIPanel,UIWidget,UIDrawCall底层原理
tagline: by 恬静的小魔龙
tag: Unity3D
---

<p>之前项目中用的NGUI的版本是3.0.7 f3，开始的时候感觉没有什么问题，直达最近项目UI的完成度比较高时，就突然出现掉帧很严重的现象，即使只有一个UI打开（其他都是active = false的情况下），打开profier，发现UIPanel LateUpdate 竟然占了CPU使用率的50%左右，这太恐怖了，虽然之前看到过有吐槽NGUI的机制的，但是我觉得为了保证通用牺牲一些性能还是在所难免的，但是没想到这个版本竟然这么废。</p>

<p>        之前虽然研究过NGUI的UIWidget, UIDrawCall,UIGeometry和 UIPanel等基础脚本（<a href="http://dsqiu.iteye.com/blog/1965340">NGUI所见即所得之UIWidget , UIGeometry &amp; UIDrawCall</a>，<a href="http://dsqiu.iteye.com/blog/1973651">NGUI所见即所得之UIPanel</a>），也大概清楚了NGUI的绘制原理。但对具体的逻辑还是不够清楚，有点凤毛麟角。为了更好的改进NGUI的性能以及更加规范使用NGUI，只有把NGUI的底层吃透。</p>

<p>        由于在之前的文章介绍了UIGeometry，UIDrawCall和UIWidget之间的关系，以及UIPanel的管理机制，所以本文主要剖析底层的原理，主要要弄清楚一下问题：</p>

<p> </p>

<p>               1. transform ,大小（size)的变化的底层绘制影响</p>

<p>               2.颜色（包括透明度）变化的底层绘制影响</p>

<p>               3.enable 和 disable 状态变化底层的处理</p>

<p>               4.UIDrawCall 和 UIPanel 机制的细节</p>

<p>       </p>

<p>        未免读者理不顺，先简单说下UIGeometry，UIDrawCall和UIWidget的关系：UIWidget是UI的基础组件（UILabel,UISprite）的基类，含有组件的基本信息（width，Height，color等），UIGeometry是UIWidget的几何数据，记录了顶点坐标，贴图的UVs和颜色等信息，UIDrawCall是将多个UIWidget的UIGeometry组合起来一起绘制，具体的UIWidget如果共用一个UIDrawCall由UIPanel控制，要想了解更多可以点击上面的链接的文章查看。</p>

<p>        虽然从人的求知欲角度，我们的疑问是按照上面 1-4 排列的，但是下面却是从 4开始介绍，只要把4理解透了3,2,1就自然迎刃而解了。</p>

<p>UIDrawCall</p>

<p>        UIGeometry相对简单，这里就不再浪费篇幅介绍了，UIDrawCall是绘制的基础组件，还是有必要仔细介绍下。</p>

<p>1.成员变量</p>

<p>        仅对几个比较重要又搞不明白的变量进行解析：</p>

<p>        a)List&lt;UIDrawCall&gt; mActiveList 和 mInactiveList ： 为什么会有两个List，mAcitveList 保持当前激活的UIDrawCall， mInactiveList主要是用于回收UIDrawCall.Destroy()的UIDrawCall，以达到循环利用避免内存的反复申请和释放，减少GC的次数。这个机制前面介绍的 vp_Timer采用这个策略。</p>

<p>        b)Material mMaterial 和 mDynamicMat：不是讲究节约内存么，怎么会有两个Material，mMaterial就是我们图集的材质Material，mDynamicMat是实际采用的Material，因为UIPanel 的 Clipping有 AlphaClipp 和 SoftClip 这两个是要通过切换Shader来实现的，所以需要对应动态创建一个Material，这个就是mDynamicMat的存在。</p>

<p>        c)bool mRebuildMat 和 isDirty：这两者表示UIDrawCall所处的状态，当改变UIDrawCall的 Material 和 Shader ，mRebuildMat就变为 true，就会引起 RebuildMaterial()的调用。isDirty若为 true ，表示UIDrawCall要进行重写“填充”，调用Set函数。</p>

<p>C#代码</p>

<p> </p>

<pre>
<code class="language-html hljs">public Material baseMaterial
    {
        get{return mMaterial;}
        set
        {
            if (mMaterial != value)
            {
                mMaterial = value;
                mRebuildMat = true;
            }
        }
    }
    public Shader shader
    {
        get{ return mShader;}
        set
        {
            if (mShader != value)
            {
                mShader = value;
                mRebuildMat = true;
            }
        }
    }</code></pre>

<p> </p>

<p>2.几个重要的函数</p>

<p>        a)CreateMaterial, RebuildMaterial 和 UpdateMaterial，这是三个后面包含前面，总之就是完成材质的创建或更新。</p>

<p>        b)Set (BetterList&lt;Vector3&gt; verts,BetterList&lt;Vector3&gt; norms,BetterList&lt;Vector4&gt; tans,BetterList&lt;Vector2&gt; uvs,BetterList&lt;Color32&gt; cols)，根据verts,norms,tans,uvs,cols重新构建Mesh，MeshRender。</p>

<pre>
<code class="language-html hljs">mMesh.vertices = verts.buffer;
mMesh.uv = uvs.buffer;
mMesh.colors32 = cols.buffer;
if (norms != null) mMesh.normals = norms.buffer;
if (tans != null) mMesh.tangents = tans.buffer;</code></pre>

<p> </p>

<p>c)OnEnable,Ondisable 和 OnDestroy：销毁了mDynamicMat，可以看出Material比Mesh更简单，不用太考虑内存问题，然后OnDestroy()没有发现调用。</p>

<pre>
<code class="language-html hljs">void OnEnable () { mRebuildMat = true; }

    void OnDisable ()
    {
        depthStart = int.MaxValue;
        depthEnd = int.MinValue;
        panel = null;
        manager = null;
        mMaterial = null;
        mTexture = null;

        NGUITools.DestroyImmediate(mDynamicMat);
        mDynamicMat = null;
    }

    void OnDestroy ()
    {
        NGUITools.DestroyImmediate(mMesh);
    }</code></pre>

<p> d)Create , Clear 和 Destroy：Create 先从mInactiveList中取出一个，在附上属性达到重复利用，Destroy是将没用的UIDrawCall从mActiveList移到mInactiveList中：</p>

<pre>
<code class="language-html hljs">static UIDrawCall Create (string name)
    {       
                //省略其他处理if (mInactiveList.size &gt; 0)
        {
            UIDrawCall dc = mInactiveList.Pop();
            mActiveList.Add(dc);
            if (name != null) dc.name = name;
            NGUITools.SetActive(dc.gameObject, true);
            return dc;
        }
                //省略其他处理
        // Create the draw call        mActiveList.Add(newDC);
        return newDC;
    }
    staticpublicvoid Destroy (UIDrawCall dc)
    {
        if (dc)
        {
            if (Application.isPlaying)
            {
                if (mActiveList.Remove(dc))
                {
                    NGUITools.SetActive(dc.gameObject, false);
                    mInactiveList.Add(dc);
                }
            }
            else
            {
                mActiveList.Remove(dc);
                NGUITools.DestroyImmediate(dc.gameObject);
            }
        }
    }</code></pre>

<p>UIPanel</p>

<p>       之前就介绍过UIPanel，也画了UIPanel主要函数的调用栈（<a href="http://dsqiu.iteye.com/blog/1973651">点击查看</a>），这里也简单罗列下LateUpdate的函数调用：</p>

<p> LateUpdate</p>

<p>      UpdateSelf</p>

<p>                UpdateTransformMatrix : 调整 worldToLocal 矩阵用于调整其管理的UIWidget的transform，并进一步调整顶点信息，还调整clipOffset的变量</p>

<p>                UpdateLayers : 更新LayerMask</p>

<p>                UpdateWidgets : 调整UIWidget</p>

<p>                            UIWidget.UpdateGeometry : 调整UIWidget的几何（顶点等）信息</p>

<p>                                              OnFill(geometry.verts, geometry.uvs, geometry.cols): 如果颜色（透明度）和大小等改变就重新填充顶点信息</p>

<p>                                              geometry.ApplyTransform : transform发生改变，调整UIGeometry中顶点的位置（矩阵计算）</p>

<p>                FillAllDrawCalls  or FillDrawCall : 重新构建所有UIDrawCall (当UIWdiget的depth发生变化），否则只调整有UIWidget的UIDrawCall</p>

<p>      UpdateDrawCalls : 调整UIPanel管理的UIDrawCall 的 transform 和 clip 等属性</p>

<p>      越来越觉得NGUI的代码组件结构越来越清晰，虽然篇幅很长（有1600多行）但理解还是可以很简单的。</p>

<p> </p>

<p>UIWidget</p>

<p>       UIWidget有一个变量 mChange 和一个函数 MarkAsChange() 很重要，这两个标记UIWidget是否变化需要进行调整的状态。</p>

<p>                1.当 Anchor , Pivot , Alpha 以及 UILabel 和 UISprite 的一些状态的改变 mChange = true ，即会调整Geometry信息</p>

<p>                2.MarkAsChange 会执行 drawCall.isDirty = true; 这样就会导致其所属的 UIDrawCall 需要重写构建 </p>

<p> </p>

<p>针对前面 1-3 的疑问进行如下总结：</p>

<p>      UIWidget（UILabel , UISprite）的任何变化（transform , drawSize , width , heigth , color , pivot ,anchor 等）变化都会引起绘制该UIWidget进行重新构建——对Mesh的顶点进行刷新，尤其是depth的变化会使得所有UIDrawCall 进行重写调整，这是非常耗性能的。</p>

<p>       </p>

<p>总结：</p>

<p>       NGUI的好处就是：合并Mesh和图集节省DrawCall，由于影响Mesh的因素太多了，所以会“牵一发而动全身”，NGUI采取的一个通用的策略，没有对不同的情况做不同的处理，都是采用某个UIDrawCall全部刷新甚至是全部UIDrawCall的刷新，这也是大家吐槽的“重中之重”。</p>

<p>       D.S.Qiu认为针对不用的情况还是会有不少优化的，比如改变alpha值，可以不需要重新调整顶点verts，而只需要单独调整cols的alpha通道，改变depth也不需要全部调整UIDrawCall，这样明显是没有做到严格的管理的。</p>

<p>       对此，D.S.Qiu提出2点使用NGUI制作UI的建议：</p>

<p>                1）尽量是UIWidget静动分离，即静止的尽量合成单独一个UIPanel，会变化的就放在另外一个UIPanel</p>

<p>                2）尽量控制UIPanel和UIDrawCall的数量，充分利用图集的空间，对“夹层”的情况可以通过图集的调整，使得UIDrawCall变得更少。</p>
