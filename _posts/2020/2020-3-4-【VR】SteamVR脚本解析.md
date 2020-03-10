---
layout: post
category: web
title: 【VR】SteamVR脚本解析
tagline: by 恬静的小魔龙
tag: Other
---

## SteamVR脚本解析

### SteamVR各脚本的功能
    
#### SteamVR/Scripts/下脚本各功能的实现


![在这里插入图片描述](//upload-images.jianshu.io/upload_images/3912830-6bb2d381e46db4ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
<blockquote><p>1、SteamVR.cs 单例管理类，管理SteamVR程序的运行和终止。<br>2、SteamVR_Camera.cs 给场景添加一个最基本可运行的SteamVR组。<br>3、SteamVR_CameraFlip.cs 使用Shader将屏幕图像反转得到最终图像。<br>4、SteamVR_CameraMask.cs 将头盔中看不到的屏幕像素遮盖。<br>5、SteamVR_Controller.cs 管理类，管理所有设备的输入控制<br>6、SteamVR_ControllerManager.cs 管理类，管理场景中的设备活动<br>7、SteamVR_Ears.cs 控制Audio Listener的方向<br>8、SteamVR_ExternalCamera.cs 用于渲染外部摄像机<br>9、SteamVR_Fade.cs 屏幕渐变功能<br>10、SteamVR_Frustum.cs 生成用于渲染的面片<br>11、SteamVR_GameView.cs 处理除眼图像之外的渲染<br>12、SteamVR_IK.cs 手柄IK的控制<br>13、SteamVR_LoadLevel.cs 用于场景之间的平滑切换<br>14、SteamVR_Menu.cs 给出一个范例菜单<br>15、SteamVR_Overlay.cs 提供和控制2D图像的绘制<br>16、SteamVR_PlayArea.cs 对移动空间的设置<br>17、SteamVR_Render.cs 控制眼图像的渲染<br>18、SteamVR_RenderModel.cs 渲染手柄模型<br>19、SteamVR_Skybox.cs 设置天空盒<br>20、SteamVR_SphericalProjection.cs 应该是应用畸变投影矩阵<br>21、SteamVR_Stats.cs 通过GUI Text显示头盔状态<br>22、SteamVR_Status.cs 由事件控制的渐变效果的基类<br>23、SteamVR_StatusText.cs 继承22的文字渐变<br>24、SteamVR_TestController.cs 测试手柄每个按钮的输入<br>25、SteamVR_TrackedCamera.cs 提供记录相机的位置的功能<br>26、SteamVR_TrackedObject.cs 使场景中的物体和控制器的Pose保持一致<br>27、SteamVR_UpdatePose.cs 当使用OpenVR接口时用此更新Pose<br>28、SteamVR_Utils.cs 一些公共方法和数据结构</p></blockquote>

### SteamVR/Extras/脚本下功能的实现
![在这里插入图片描述](//upload-images.jianshu.io/upload_images/3912830-1d09a67b50051b4e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<blockquote><p>SteamVR_GazeTracker.cs 提供凝视时的事件<br>SteamVR_LaserPointer.cs 应该是镭射光线<br>SteamVR_Teleporter.cs 传送功能<br>SteamVR_TestThrow.cs 投掷东西<br>SteamVR_TestTrackedCamera.cs 跟踪相机测试<br>SteamVR_TrackedController.cs 手柄按钮事件的接口</p></blockquote>
<h4>详细脚本解析：</h4>
<p><strong><em>SteamVR_GazeTracker.cs脚本解析</em></strong><br>这个脚本的作用是判断当前物体是否被用户（头显）所注视，进入注视和离开注视都会有回调。处于注视状态的物体与实际注视点的距离范围定义为小于0.15米，而离开注视状态的距离范围为大于0.4米。之所以有一个大概的范围，并且使用了一个平面来相交，是因为注视这个动作是比较粗略的，玩家比较难能精确注视。<br>　　Gaze回调的事件结构体，只有一个参数，即距离，表示凝视点与物体（中心）的距离</p>

```csharp
public struct GazeEventArgs
{
    public float distance;
}

public delegate void GazeEventHandler(object sender, GazeEventArgs e);

public class SteamVR_GazeTracker : MonoBehaviour
{
    //当前是否处于gaze状态
    public bool isInGaze = false;
    //入gaze状态回调，使用者可以通过代码添加自己的事件处理方法（在Inspector      中不会出现）
    public event GazeEventHandler GazeOn;
    //离开gaze状态回调
    public event GazeEventHandler GazeOff;
    //定义的进入gaze与离开gaze的距离范围
    public float gazeInCutoff = 0.15f;
    public float gazeOutCutoff = 0.4f;

    // Contains a HMD tracked object that we can use to find the user's gaze
    //头显的transform对象
    Transform hmdTrackedObject = null;
    // Use this for initialization
    void Start ()
    {

    }
    public virtual void OnGazeOn(GazeEventArgs e)
    {
        //如果有注册GazeOff回调，调用它
        if (GazeOn != null)
            GazeOn(this, e);
    }
    public virtual void OnGazeOff(GazeEventArgs e)
    {

        //如果有注册GazeOff回调，调用它
        if (GazeOff != null)
            GazeOff(this, e);
    }
    // Update is called once per frame
    void Update ()
    {
        // If we haven't set up hmdTrackedObject find what the user is looking at
        if (hmdTrackedObject == null)
        {
            //首次调用会去查找头显，方法是查找所有SteamVR_TrackedObject对象。所有的跟踪对象（比如头显、手柄、基站）都是SteamVR_TrackedObject对象（相应的对象上附加了SteamVR_TrackedObject脚本）
            SteamVR_TrackedObject[] trackedObjects = FindObjectsOfType&lt;SteamVR_TrackedObject&gt;();
            foreach (SteamVR_TrackedObject tracked in trackedObjects)
            {
                if (tracked.index == SteamVR_TrackedObject.EIndex.Hmd)
                {
                    //找到头显设备，取其transform对象。头显设备的索引是0号索引
                    hmdTrackedObject = tracked.transform;
                    break;
                }
            }
        }
        if (hmdTrackedObject)
        {
            //构造一条从头显正方向的射线
            Ray r = new Ray(hmdTrackedObject.position, hmdTrackedObject.forward);
            //构造一个头显正方向、在当前物体位置的平面
            Plane p = new Plane(hmdTrackedObject.forward, transform.position);

            float enter = 0.0f;
            //射线与物体平面正向相交，返回的enter为沿射线的距离。如果不相交，或者反向相交，则下面的Raycast返回false
            if (p.Raycast(r, out enter))
            {
                //intersect为射线与物体平面在三维空间的交点
                Vector3 intersect = hmdTrackedObject.position + hmdTrackedObject.forward * enter;
                //计算空间两点的距离，即物体当前位置与交点的距离
                float dist = Vector3.Distance(intersect, transform.position);
                //Debug.Log("Gaze dist = " + dist);
                if (dist &lt; gazeInCutoff &amp;&amp; !isInGaze)
                {
                    //当前物体与凝视点的距离小于0.15米，则认为进入gaze状态
                    isInGaze = true;
                    GazeEventArgs e;
                    e.distance = dist;
                    OnGazeOn(e);
                }
                else if (dist &gt;= gazeOutCutoff &amp;&amp; isInGaze)
                {
                    //当前物体与凝视点的距离超过0.4米，则认为离开gaze状态
                    isInGaze = false;
                    GazeEventArgs e;
                    e.distance = dist;
                    OnGazeOff(e);
                }
            }
        }
    }
}
```


<h4>SteamVR_LaserPointer.cs</h4>
<p>　这个脚本的作用与上面的SteamVR_GazeTracker相关及类似。GazeTracker是通过头显的正视方向与物体相交来计算交点的。而这里是通过所谓的激光束来与物体相交的。激光束就是手柄指向的方向，可以在游戏里面把这个方向渲染出一条激光束出来，特别是在通过手柄进行菜单的UI操作的时候。在github openvr的sample目录下的unity_teleport_sample示例有使用，它被加到右手柄上<br>　同上面的GazeTracker一样，触发的事件所带的参数</p>

```csharp
public struct PointerEventArgs
{
    //控制器（手柄）索引
    public uint controllerIndex;
    //目前好像并没有用到
    public uint flags;
    //激光原点到命中点（交点）的距离
    public float distance;
    //命中物体的transform对象
    public Transform target;
}
public delegate void PointerEventHandler(object sender, PointerEventArgs e);
public class SteamVR_LaserPointer : MonoBehaviour
{
    //这个变量并未使用
    public bool active = true;
    //　激光的颜色
    public Color color;
    //激光束的粗细（创建了一个立方体，按下面的scale，x、y是0.002，z是100，就能看 到是一条很长的细线了）
    public float thickness = 0.002f;
    //一个空的GameObject，用于作激光束的parent
    public GameObject holder;
    //激光束本身，是用一个立方体拉长来模拟的（为啥不用圆柱体？显然立方体要比圆柱体渲染简单得多，在很细的情况下，用立方体是明智的选择）
    public GameObject pointer;
    //用来判断是否为第一次调用
    bool isActive = false;
    //这个是暴露在inspector中的属性，用于控制是否给激光束（长方体）添加刚体。本身光是没有重量的，没有必要添加刚体吧。所以这里缺省是false
    public bool addRigidBody = false;
    //这个变量并未使用
    public Transform reference;
    //同上面的GazeTracker一样，用于触发激光命中和离开事件
    public event PointerEventHandler PointerIn;
    public event PointerEventHandler PointerOut;
    //上次激光命中的物体的transform对象，用于判断是否命中同一个物体
    Transform previousContact = null;


    // Use this for initialization
    void Start ()
    {
        //在脚本被加载的时候，做一些初始化
        //首先创建一个holder（即激光束的父物体）
        holder = new GameObject();
        //holder的transform的parent设为当前脚本所在的物体（通常这个脚本会加到控制器手柄上面）
        holder.transform.parent = this.transform;
        //位置设在0点（本地坐标系，相对于父亲）
        holder.transform.localPosition = Vector3.zero;
        holder.transform.localRotation = Quaternion.identity;
        //创建激光束，用长方体模拟
        pointer = GameObject.CreatePrimitive(PrimitiveType.Cube);
        //将父亲设为上面的holder
        pointer.transform.parent = holder.transform;
        //设置locale为(0.002,0.002,100)，看起来就是一条很长的线
        pointer.transform.localScale = new Vector3(thickness, thickness, 100f);
        //位置设在父亲的(0,0,50)位置，因为对于立方体(长方体)，其中心在立方体中心，因为上面被放大到了100倍，那移动位置到(0,0,50)可以让激光束的起点为父亲
        pointer.transform.localPosition = new Vector3(0f, 0f, 50f);

        pointer.transform.localRotation = Quaternion.identity;
        // 如果指定了addRigidBody为true，则为激光束添加一个刚体，对应的collider 则只设为触发器（不会执行碰撞，但会进入代码）。否则，会把collider销毁掉，也就是不需要collider
        BoxCollider collider = pointer.GetComponent&lt;BoxCollider&gt;();
        if (addRigidBody)
        {
            if (collider)
            {
                collider.isTrigger = true;
            }
            Rigidbody rigidBody = pointer.AddComponent&lt;Rigidbody&gt;();
            rigidBody.isKinematic = true;
        }
        else
        {
            if(collider)
            {
                Object.Destroy(collider);
            }
        }
        //新建纯色材质并添加到MeshRender中。Color值通过inspector设置
        Material newMaterial = new Material(Shader.Find("Unlit/Color"));
        newMaterial.SetColor("_Color", color);
        pointer.GetComponent&lt;MeshRenderer&gt;().material = newMaterial;
    }

    public virtual void OnPointerIn(PointerEventArgs e)
    {
        //回调激光命中委托
        if (PointerIn != null)
            PointerIn(this, e);
    }

    public virtual void OnPointerOut(PointerEventArgs e)
    {
        //回调激光不再命中委托
        if (PointerOut != null)
            PointerOut(this, e);
    }


    // Update is called once per frame
    void Update()
    {
        if (!isActive)
        {
            //第一次调用时将holder设为active（当前物体transform的第一个child就是holder）
            isActive = true;
            this.transform.GetChild(0).gameObject.SetActive(true);
        }
        //命中物体（或者说激光束）的最远距离记为100米
        float dist = 100f;
        //当前物体（手柄上）上还要挂一个SteamVR_TrackedController脚本
        SteamVR_TrackedController controller = GetComponent&lt;SteamVR_TrackedController&gt;();
        //  构造一条射线
        Ray raycast = new Ray(transform.position, transform.forward);
        RaycastHit hit;
        //计算射线命中的场景中的物体
        bool bHit = Physics.Raycast(raycast, out hit);

        if(previousContact &amp;&amp; previousContact != hit.transform)
        {
            // 如果之前已经有一个命中的物体，而当前命中的物体发生了变化，那么说明前一个命中的物体就要收到一个不再命中的通知
            PointerEventArgs args = new PointerEventArgs();
            if (controller != null)
            {
                args.controllerIndex = controller.controllerIndex;
            }
            args.distance = 0f;
            args.flags = 0;
            args.target = previousContact;
            OnPointerOut(args);
            previousContact = null;

        }
        if(bHit &amp;&amp; previousContact != hit.transform)
        {
            //通知命中新的物体
            PointerEventArgs argsIn = new PointerEventArgs();
            if (controller != null)
            {
                argsIn.controllerIndex = controller.controllerIndex;
            }
            // hit.distance为射线原点到命中点的距离
            argsIn.distance = hit.distance;
            argsIn.flags = 0;
            //target记录的是命中物体的transform
            argsIn.target = hit.transform;
            OnPointerIn(argsIn);
            // 记录上一次命中的物体的transform
            previousContact = hit.transform;
        }
        if(!bHit)
        {
            previousContact = null;
        }
        if (bHit &amp;&amp; hit.distance &lt; 100f)
        {
            //如果命中物体距离小于100，则记录下来，否则最远就是100米
            dist = hit.distance;
        }

        if (controller != null &amp;&amp; controller.triggerPressed)
        {
            //当按下扳机键时，将光束的粗细增大5倍，同时长度会设为dist，这样看起来光束就会到命中点截止，不会穿透物体
            pointer.transform.localScale = new Vector3(thickness * 5f, thickness * 5f, dist);
        }
        else
        {
            //按下扳机或者当前控制器没有添加SteamVR_TrackedController时，显示原始粗细的光束
            pointer.transform.localScale = new Vector3(thickness, thickness, dist);
        }
        //光束的位置总是设在光束长度的一半的位置，使得光束看起来总是从手柄发出来的
        pointer.transform.localPosition = new Vector3(0f, 0f, dist/2f);
    }
}
```

<h4>SteamVR_Teleporter.cs</h4>
<p>这个脚本在github上的openvr的samples里面的unity_teleport_sample示例中有使用，从名字上看是用来做瞬移的。这个示例就是在场景中放了一些球，然后在右控制器上放了这个脚本，也就是可以通过右控制器做瞬移。做法是会从控制器上发出一个激光束，按下扳机键就可以瞬移到激光束指到的地方。通过观察球的相对位置就能看到位置的变化。这在游戏里面很常见，拿鼠标一点，被操作的对象就会过去（当然过去有两种方式，一种是慢慢走过去，另一种就是直接跳过去），另一个见过的就是街景地图。</p>

```csharp
{
　　public enum TeleportType
　　{
　　//瞬移类型是根据与地形的交点来确定目的位置的
　　TeleportTypeUseTerrain,
　　//碰撞体类型是根据与任何带碰撞体的物体的交点来确定目的位置的
　　TeleportTypeUseCollider,
　　//这个是与Y坐标为0的平面（通常就是地面）的交点来确定目的位置的
　　TeleportTypeUseZeroY
　　}
　　public bool teleportOnClick = false;
　　public TeleportType teleportType = TeleportType.TeleportTypeUseZeroY;
　　Transform reference
　　{
　　get
　　{
　　//取的是最后渲染（depth最大）的相机（SteamVR_Camera）的原始点（这个origin实际就是将SteamVR_Camera添加到的原始场景中的Camera的位置）
　　var top = SteamVR_Render.Top();
　　//SteamVR相机的层次结构是最上层是origin，然后下面有左右手柄和head，head下面有eye和ears
　　return (top != null) ? top.origin: null;
　　}
　　}
　　void Start ()
　　{
//这个脚本所在的物体必要要添加SteamVR_TrackedController脚本（这个脚本的作用是将控制器的输入转换为事件回调），如果没有添加，则自动添加。这说明这个脚本所在的物体需要是控制器（手柄），在unity_teleport_sample示例中，正是加到了右手柄上
 　　var trackedController = GetComponent();
　　if (trackedController == null)
　　{
　　trackedController= gameObject.AddComponent();
　　}
       // 添加扳机按下的回调

　　trackedController.TriggerClicked+= new ClickedEventHandler(DoClick);
　　if (teleportType== TeleportType.TeleportTypeUseTerrain)
　　{
　　// Start theplayer at the level of the terrain
　　var t =reference;
　　if (t != null)
                //如果是地形类型，会将相机origin（基本上可以认为就是玩家的位置）的Y坐标先调整为地形的采样高度（即相机origin所在位置的地形的实际Y坐标——即将相机origin放到地形表面——也就是在地形表面的垂直投影的位置），这样可以避免人钻到地型里面了

　　t.position = new Vector3(t.position.x, Terrain.activeTerrain.SampleHeight(t.position), t.position.z);
　　}
　　}
　　void DoClick(object sender, ClickedEventArgs e)
　　{
       //应该是通过这个变量来控制是否通过扳机键来瞬移。因为扳机键还可以用作其它用途，应该是在某种状态下才能通过扳机键来瞬移。比如，需要通过扳机键来瞬移时才需要将这个变量设为true

　　if (teleportOnClick)
　　{
　　var t =reference;
　　if (t == null)
　　return;
　　float refY =t.position.y;
}</code></pre>
<p>创建了一个Y方向，-refY位置的平面（是-refY而不是refY的原因是这个是Plane的distance参数，而Plane的distance是原点到Plane的距离，而距离的正负决定了在平面的哪一边，为正表示原点在法线的正方向，为负表示原点在法线的反方向，这与通常的理解不一样，所以这里为-refY）<br></p><div class="image-package">
<img src="//upload-images.jianshu.io/upload_images/3912830-e4c8d47d4bfa1c35.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/3912830-e4c8d47d4bfa1c35.png?imageMogr2/auto-orient/strip%7CimageView2/2"><br><div class="image-caption">Paste_Image.png</div>
</div>
<pre><code>Plane plane = new Plane(Vector3.up, -refY);</code></pre>
<pre><code>//  当前脚本应该绑定在手柄上，因此才会有手柄方向的一条射线
Ray ray = new Ray(this.transform.position,transform.forward);
// hasGroudTarget是指是否射线与地面相交，或者说是否射到了地面上
bool hasGroundTarget = false;
// dist为射线原点（即手柄的原点）与相交点的距离

float dist = 0f;
　　if (teleportType== TeleportType.TeleportTypeUseTerrain)
　　{
                //与地形进行碰撞

　　RaycastHit hitInfo;
　　TerrainCollider tc = Terrain.activeTerrain.GetComponent();
　　hasGroundTarget = tc.Raycast(ray, out hitInfo, 1000f);
　　dist = hitInfo.distance;
　　}
　　else if (teleportType== TeleportType.TeleportTypeUseCollider)
　　{
                //与场景中的碰撞体进行碰撞

　　RaycastHit hitInfo;
　　Physics.Raycast(ray, out hitInfo);
　　dist = hitInfo.distance;
//这里并没有设为hasGroundTarget为true，那后面的瞬移就无法完成。所以设置为TeleportTypeUseCollider应该就不能瞬移啊（实测确实不可以），那为什么要设置这种类型？
//从实际意义来说，确实是不能你扳 机指向哪就瞬移到哪，人不是什么地方都能去的

　　}
　　else
　　{
                //与地面（Y方向的一个平面）相交

　　hasGroundTarget = plane.Raycast(ray, out dist);
　　}
　　if (hasGroundTarget)
　　{
              // headPosOnGround是head（head就是头显的位置）在地面（Y=0）的投 影，注意这里用的是localPosition

　　Vector3 headPosOnGround = new Vector3(SteamVR_Render.Top().head.localPosition.x, 0.0f, SteamVR_Render.Top().head.localPosition.z);
 //这里就是将origin移动到扳机位置了。ray.origin + ray.direction*dist得到的就是射线与地形/地面交点的位置。后面减去的两个点分别是手柄（这里取的是第一个子物体，实际上就是左控制器）和
//头显相对于origin的位置(XZ平面)。感觉没有必要减，按照所见即所得，玩家看到的激光束的交点，就直接把位置定到那就好了，不需要考虑头显或者手柄的偏移

　　t.position= ray.origin + ray.direction * dist - new Vector3(t.GetChild(0).localPosition.x, 0f, t.GetChild(0).localPosition.z)- headPosOnGround;
　　 }
　 }
 }
}
```

<h4>SteamVR_TestIK.unity</h4>
<p>这是一个测试IK（更准确地说应该是SteamVR_IK.cs的，反向运动，就是根据手柄的运动模拟带动手臂的运动）的示例场景。这个场景里有一个模拟手臂：</p>
![在这里插入图片描述](//upload-images.jianshu.io/upload_images/3912830-b8cfc512a2341e7b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
</div><p>分左右手，分别在左右两个手柄控制器下面，在场景中的样子是这样的：<br></p>
![在这里插入图片描述](//upload-images.jianshu.io/upload_images/3912830-059a1ee3092f2c32.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
</div><p>它这里只有两个关节（肩关节和腕关节，SteamVR_IK就只支持两个关节），然后有一个手指</p>

<h4>SteamVR_TestThrow.unity</h4>
<p>这个应该是测试通过手柄扔出一个物体的例子，主要是测试下面的这个SteamVR_TestThrow脚本，可以看到这个脚本被添加到了左右两个手柄上面：<br></p><div class="image-package">
![在这里插入图片描述](//upload-images.jianshu.io/upload_images/3912830-291fd9ceeb38c4ac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
</div><p>Template这个物体是被扔的物体，它由一个圆及一个子物体立方体组成。通过扳机键创建一个物体并抓在手中，然后通过甩臂并同时松开扳机将物体扔出去</p>

<h4>SteamVR_TestThrow.cs</h4>
<p>这个脚本上面的测试场景的控制脚本，要与SteamVR_TrackedObject一起使用。实际上它会加到手柄上。</p>

```csharp
[RequireComponent(typeof(SteamVR_TrackedObject))]

　　public&gt;　　{
　　//要扔掉的物体，并不是一个真正的prefab，而是一个场景中已经创建好的物体
　　public GameObject prefab;
　　//这个是手柄上tip（手柄模型的一部分）下面的一个rigidbody。在手柄的模型中，所有的子组件（相对于父组件，即整个model）的位置都是(0,0,0)，
//但下面会再带一   个attach的子对象，这个子对象是一个刚体，然后真正的位置是通过它来确定的
　　public Rigidbody attachPoint;
　　//手柄这个跟踪对象
　　SteamVR_TrackedObject trackedObj;
　　//固定关节
　　FixedJoint joint;
　　void Awake()
　　{
   //Awake不管脚本是不是启用都会调用

　　trackedObj= GetComponent();
　　}
　　void FixedUpdate()
　　{
   // 返回的Device对象（SteamVR_Controller内部类），对当前跟踪设备的输入进行了一些封装

　　var device = SteamVR_Controller.Input((int)trackedObj.index);
　　if (joint == null &amp;&amp;device.GetTouchDown(SteamVR_Controller.ButtonMask.Trigger))
　　{
        //如果还没有建立关节，当按下扳机键时，建立物体与手柄的（关节）关联，相当于就是抓起了物体

       // 先创建了物体（复制了场景中的一个物体）

　　var go = GameObject.Instantiate(prefab);
        //创建的物体的位置位于tip的关联点的位置。

　　go.transform.position= attachPoint.transform.position;
        //添加固定关节，这样物体就能跟随手柄动而动了

　　joint= go.AddComponent();
        //将其与手柄tip的attach关联

　　joint.connectedBody= attachPoint;
　　}
　　else if (joint != null &amp;&amp;device.GetTouchUp(SteamVR_Controller.ButtonMask.Trigger))
　　{
        //而如果已经建立了关节，再按扳机键时则会销毁关节，由于物体是一个刚体，就会自由下落。当然下面还会通过手柄给它一个初速度

　　var go =joint.gameObject;
　　var rigidbody =go.GetComponent();
        //销毁关节

　　Object.DestroyImmediate(joint);
　　joint= null;
       //15秒后销毁物体

　　Object.Destroy(go, 15.0f);
　　// We shouldprobably apply the offset between trackedObj.transform.position
　　// anddevice.transform.pos to insert into the physics sim at the correct
　　// location,however, we would then want to predict ahead the visual representation
　　// by the sameamount we are predicting our render poses.
       /origin是SteamVR_TrackedObject中的一个变量，它大概就是SteamVR_Camera中的origin，也就是CameraRig的顶层物体，基本上它可以代表的是玩家的身体。
//因为除了头部（头显）及手臂（手柄）会动以外，身体本身也可以动。如果没有指定，则直接使用父亲的transform

　　var origin =trackedObj.origin ? trackedObj.origin : trackedObj.transform.parent;
　　if (origin != null)
　　{
           //如果指定了origin，因为是相对坐标（速度），转换成世界坐标（速度）

　　rigidbody.velocity= origin.TransformVector(device.velocity);
　　rigidbody.angularVelocity= origin.TransformVector(device.angularVelocity);
　　}
　　else
　　{
            //如果没有指定，则直接使用跟踪设备的速度

　　rigidbody.velocity= device.velocity;
　　rigidbody.angularVelocity= device.angularVelocity;
　　}
　　rigidbody.maxAngularVelocity= rigidbody.angularVelocity.magnitude;
　　}
　　}
　　}
```

<h4>SteamVR_TrackedController.cs</h4>
<p>这个脚本对控制器的输入做了一个简单的封装，将原始的输入数据转化成事件回调模式，它在SteamVR_LaserPointer.cs和SteamVR_Teleporter.cs中有使用，也就是在github上openvr中的teleporter示例中有使用。在SteamVR_Teleporter.cs中，如果当前物体上没有SteamVR_TrackedController.cs，会自动添加它（在老版的插件中，如果没有添加，会报错）。直接看代码看它干了些什么：<br>　　点击事件回调参数</p>

```csharp
public struct ClickedEventArgs
　　{
　　public uint controllerIndex;
　　public uint flags;
　　public float padX, padY;
　　}
　　//点击事件处理委托方法
　　public delegate void ClickedEventHandler(object sender, ClickedEventArgs e);
　　public&gt;　　{
　　//控制器索引，即跟踪设备的索引
　　public uint controllerIndex;
　　//控制器的状态，比如按键是否按下，轴数据等
　　public VRControllerState_t controllerState;
　　//是否按了扳机键
　　public bool triggerPressed = false;
　　//是否按了steam键？代码中并没有使用
　　public bool steamPressed= false;
　//是否按了菜单键
　　public bool menuPressed = false;
　　//是否在触控板上按下了
　　public bool padPressed = false;
　　//是否在触控板上触控
　　public bool padTouched = false;
　　//是否按下了拾取键
　　public bool gripped = false;
　　//各种按键的回调方法
　　public event ClickedEventHandler MenuButtonClicked;
　　public event ClickedEventHandler MenuButtonUnclicked;
　　public event ClickedEventHandler TriggerClicked;
　　public event ClickedEventHandler TriggerUnclicked;
　　public event ClickedEventHandler SteamClicked;
　　public event ClickedEventHandler PadClicked;
　　public event ClickedEventHandler PadUnclicked;
　　public event ClickedEventHandler PadTouched;
　　public event ClickedEventHandler PadUntouched;
　　public event ClickedEventHandler Gripped;
　　public event ClickedEventHandler Ungripped;
　　// Use this for initialization
　　void Start()
　　{
　　//如果当前物体上没有添加SteamVR_TrackedObject，则自动添加
　　if (this.GetComponent() == null)
　　{
　　gameObject.AddComponent();
　　}
　　if (controllerIndex != 0)
　　{
　　//设置跟踪设备的索引
　　this.GetComponent().index =(SteamVR_TrackedObject.EIndex)controllerIndex;
　　if (this.GetComponent() != null)
　　{
     //如果当前物体（跟踪设备）上还有SteamVR_RenderModel，也设置它的索引。在SteamVR的Unity插件中，控制器本身上面并没有SteamVR_RenderModel，
//但在其下面的Model上面有，然后就是其它的跟踪设备（除hmd外）上都有。

　　this.GetComponent().index =(SteamVR_TrackedObject.EIndex)controllerIndex;
　　}
　　}
　　else
　　{
 //如果没有（通过inspector）指定控制器索引，则从SteamVR_TrackedObject中取索引。SteamVR插件的实际情况是，就只是有左右两个控制器的 SteamVR_TrackedObject是没有指定索引的。但会在
//SteamVR_ControllerManager（通常挂在CameraRig顶层上面）中根据实际控制器的索引（比如可能只有一个控制器连接了）设置

　　controllerIndex= (uint) this.GetComponent().index;
　　}
　　}
//还提供了接口设置控制器索引，这个也是会被SteamVR_ControllerManager广播调  用的

　　public void SetDeviceIndex(int index)
　　{
　　this.controllerIndex= (uint) index;
　　}
//下面就是一些回调调用了

　　public virtual void OnTriggerClicked(ClickedEventArgs e)
　　{
　　if (TriggerClicked != null)
　　TriggerClicked(this, e);
　　}
　　public virtual void OnTriggerUnclicked(ClickedEventArgs e)
　　{
　　if (TriggerUnclicked != null)
　　TriggerUnclicked(this, e);
　　}
　　public virtual void OnMenuClicked(ClickedEventArgs e)
　　{
　　if (MenuButtonClicked != null)
　　MenuButtonClicked(this, e);
　　}
　　public virtual void OnMenuUnclicked(ClickedEventArgs e)
　　{
　　if (MenuButtonUnclicked != null)
　　MenuButtonUnclicked(this, e);
　　}
　　public virtual void OnSteamClicked(ClickedEventArgs e)
　　{
　　if (SteamClicked!= null)
　　SteamClicked(this, e);
　　}
　　public virtual void OnPadClicked(ClickedEventArgs e)
　　{
　　if (PadClicked!= null)
　　PadClicked(this, e);
　　}
　　public virtual void OnPadUnclicked(ClickedEventArgs e)
　　{
　　if (PadUnclicked!= null)
　　PadUnclicked(this, e);
　　}
　　public virtual void OnPadTouched(ClickedEventArgs e)
　　{
　　if (PadTouched!= null)
　　PadTouched(this, e);
　　}
　　public virtual void OnPadUntouched(ClickedEventArgs e)
　　{
　　if (PadUntouched!= null)
　　PadUntouched(this, e);
　　}
　　public virtual void OnGripped(ClickedEventArgs e)
　　{
　　if (Gripped != null)
　　Gripped(this, e);
　　}
　　public virtual void OnUngripped(ClickedEventArgs e)
　　{
　　if (Ungripped != null)
　　Ungripped(this, e);
　　}
　　// Update is called once per frame
　　void Update()
　　{
  // OpenVR.System即IVRSystem接口或者CVRSystem类

　　var system = OpenVR.System;
  // Update是每帧调用，而IVRSystem.GetControllerState是获取即时的指定索引的控制器的状态

　　if (system != null &amp;&amp;system.GetControllerState(controllerIndex, ref controllerState))
　　{
　　ulong trigger =controllerState.ulButtonPressed &amp; (1UL  0L &amp;&amp;!triggerPressed)
　　{
            //按下了扳机键

　　triggerPressed = true;
　　ClickedEventArgs e;
　　e.controllerIndex = controllerIndex;
　　e.flags = (uint)controllerState.ulButtonPressed;
           //padX/padY取的是轴输入设备TrackPad的x、y值。根据结构体的定义， rAxis0其实就是TrackPad

　　e.padX = controllerState.rAxis0.x;
　　e.padY = controllerState.rAxis0.y;
　　OnTriggerClicked(e);
　　}
　　else if (trigger == 0L &amp;&amp;triggerPressed)
　　{
            //松开了扳机键

　　triggerPressed = false;
　　ClickedEventArgs e;
　　e.controllerIndex = controllerIndex;
　　e.flags = (uint)controllerState.ulButtonPressed;
　　e.padX = controllerState.rAxis0.x;
　　e.padY = controllerState.rAxis0.y;
　　OnTriggerUnclicked(e);
　　}
　　ulong grip =controllerState.ulButtonPressed &amp; (1UL  0L &amp;&amp;!gripped)
　　{
            //按下了拾取键

　　gripped = true;
　　ClickedEventArgs e;
　　e.controllerIndex = controllerIndex;
　　e.flags = (uint)controllerState.ulButtonPressed;
　　e.padX = controllerState.rAxis0.x;
　　e.padY = controllerState.rAxis0.y;
　　OnGripped(e);
　　}
　　else if (grip == 0L &amp;&amp;gripped)
　　{
            //松开了拾取键

　　gripped = false;
　　ClickedEventArgs e;
　　e.controllerIndex = controllerIndex;
　　e.flags = (uint)controllerState.ulButtonPressed;
　　e.padX = controllerState.rAxis0.x;
　　e.padY = controllerState.rAxis0.y;
　　OnUngripped(e);
　　}
　　ulong pad =controllerState.ulButtonPressed &amp; (1UL  0L &amp;&amp;!padPressed)
　　{
           //在TrackPad上按下了

　　padPressed = true;
　　ClickedEventArgs e;
　　e.controllerIndex = controllerIndex;
　　e.flags = (uint)controllerState.ulButtonPressed;
　　e.padX = controllerState.rAxis0.x;
　　e.padY = controllerState.rAxis0.y;
　　OnPadClicked(e);
　　}
　　else if (pad == 0L &amp;&amp;padPressed)
　　{
            //从TrackPad上松开了

　　padPressed = false;
　　ClickedEventArgs e;
　　e.controllerIndex = controllerIndex;
　　e.flags = (uint)controllerState.ulButtonPressed;
　　e.padX = controllerState.rAxis0.x;
　　e.padY = controllerState.rAxis0.y;
　　OnPadUnclicked(e);
　　}
　　ulong menu =controllerState.ulButtonPressed &amp; (1UL  0L &amp;&amp;!menuPressed)
　　{
            //按下了菜单键

　　menuPressed = true;
　　ClickedEventArgs e;
　　e.controllerIndex = controllerIndex;
　　e.flags = (uint)controllerState.ulButtonPressed;
　　e.padX = controllerState.rAxis0.x;
　　e.padY = controllerState.rAxis0.y;
　　OnMenuClicked(e);
　　}
　　else if (menu == 0L &amp;&amp;menuPressed)
　　{
           // 松开了菜单键

　　menuPressed = false;
　　ClickedEventArgs e;
　　e.controllerIndex = controllerIndex;
　　e.flags = (uint)controllerState.ulButtonPressed;
　　e.padX = controllerState.rAxis0.x;
　　e.padY = controllerState.rAxis0.y;
　　OnMenuUnclicked(e);
　　}
　　pad =controllerState.ulButtonTouched &amp; (1UL  0L &amp;&amp;!padTouched)
　　{
            //在TrackPad上触摸

　　padTouched = true;
　　ClickedEventArgs e;
　　e.controllerIndex = controllerIndex;
　　e.flags = (uint)controllerState.ulButtonPressed;
　　e.padX = controllerState.rAxis0.x;
　　e.padY = controllerState.rAxis0.y;
　　OnPadTouched(e);
　　}
　　else if (pad == 0L &amp;&amp;padTouched)
　　{
           // 在TrackPad上取消触摸

　　padTouched = false;
　　ClickedEventArgs e;
　　e.controllerIndex = controllerIndex;
　　e.flags = (uint)controllerState.ulButtonPressed;
　　e.padX = controllerState.rAxis0.x;
　　e.padY = controllerState.rAxis0.y;
　　OnPadUntouched(e);
　　 }
　　}
　}
}
```

<p>其他参考资料：<br><a href="http://blog.csdn.net/qq_34113388/article/details/54343279" target="_blank">HTC VIVE的交互（凝视，瞬移，发出射线）</a><br> <a href="http://www.taidous.com/forum.php?mod=viewthread&amp;tid=57193_" target="_blank">SteamVR(HTC Vive) Unity插件深度分析</a></p>         
</html>
