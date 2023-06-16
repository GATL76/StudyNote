# 基本知识

​	脚本是附加在游戏物体上用于定义游戏对象行为的指令代码.

## Unity编辑器

### 语法结构

```C#
using namespace;//namespace代表命名空间
public class Name : MonoBehaviour{
	void Method(){
		Debug.Log("调试显示信息");
		print("本质就是Debug.Log方法");
	}
}
//文件名与类名必须一致
//写好的脚本必须附加到物体上才能执行
//附加到游戏物体的脚本类必须从MonoBehaviour类集成
//Unity中可以不需要对名称空间的定义
```

### 修改Unity默认脚本模板

> I:\Unity\Unity3D\2021.3.22f1c1\Editor\Data\Resources\Script Templates

```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

    #ROOTNAMESPACEBEGIN#
public class #SCRIPTNAME# : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        #NOTRIM#
    }

    // Update is called once per frame
    void Update()
    {
        #NOTRIM#
    }
}
#ROOTNAMESPACEEND#
```

### 修改unity默认脚本编辑器

> Edit=Preferences-External Tools-External Script Editor

## 脚本生命周期

​	Unity脚本从唤醒到销毁的过程.详情参考[这里](https://docs.unity3d.com/Manual/ExecutionOrder.html)

​	消息:当满足某种条件Unity引擎自动调用的函数.

### 初始阶段

* Awake唤醒:

  当物体载入时立即调用1次;常用于在游戏开始前进行初始化,可以判断当满足某种条件执行此脚本this.enable=true.

* OnEnable 当可用:

  每当脚本对象启用时调用.

* Start 开始:

  物体载入且脚本对象启用时被调用1次.常用于数据或游戏逻辑初始化,执行时机晚于Awake.
  
  ```C#
  using System.Collections;
  using System.Collections.Generic;
  using UnityEngine;
  
  /// <summary>
  ///
  /// </summary>
  
  public class Base : MonoBehaviour
  {
  //Initial
  	//Awake
      private void Awake(){
          Debug.Log("Awake-"+Time.time);
      }
      //Start
      private void Start(){
          Debug.Log("Start--"+Time.time);
      }
  }
  ```
  
  

### 物理阶段

* FixedUpdate 固定更新:

  脚本启用后,固定时间被调用,适用于对游戏对象做物理操作,例如移动等.(设置更新频率:Edit-Project Setting-Time-Fixed Timestep).该频率不会受到渲染的影响.渲染时间受到每帧渲染量,物理性能影响.

* OnCollisionXXX 碰撞:

  当满足碰撞条件时调用.

* OnTriggerXXX 触发:

  当满足触发条件时调用.



### 游戏逻辑

* Update 更新:

  脚本启用后,每次渲染场景时调用,频率与设备性能及渲染量有关.常用于处理游戏逻辑.

* LateUpdate 延迟更新:

  在Update函数被调用后执行,适用于跟随逻辑



### 输入事件

Collider:碰撞器

* OnMouseEnter 鼠标移入:

  鼠标移入到当前Collider时调用.

* OnMouseOver 鼠标经过:

  鼠标经过当前Collider时调用.

* OnMouseExit 鼠标离开:

  鼠标离开当前Collider时调用.

* OnMouseDown 鼠标按下:

  鼠标按下当前Collider时调用.

* OnMouseUp 鼠标抬起:

  鼠标在当前Collider上抬起时调用.



### 场景渲染

* OnBecameVisible 当可见:

  当Mesh Renderer在任何相机上可见时调用.

* OnBecameInvisible 当不可见:

  当Mesh Renderer 在任何相机上都不可见时调用.



### 结束阶段

* OnDIsable 当不可用:

  对象变为不可用或附属游戏对象非激活状态时此函数被调用.

* OnDestroy 当销毁:

  当脚本销毁或附属的游戏对象被销毁时被调用.

* OnApplicationQuit 当程序结束:

  应用程序退出时被调用.



## 调试

### 使用Unity编辑器

* 将程序投入到实际运行中,通过开发工具进行测试,修正逻辑错误的过程.
* 控制台调试:Debug.Log(变量) or print(变量).
* 定义共有变量,程序运行后在检测面板查看数据

### 使用外部编译器

* 添加断点
* 启动调试
* Unity中Play场景

#### Vscode中调试Unity

* 在官方文档中查找Unity,找到[这个](https://code.visualstudio.com/docs/other/unity),按步骤逐一完成
* 参照[这个视频](https://www.bilibili.com/video/BV14v4y1m7o8/?spm_id_from=333.1007.top_right_bar_window_history.content.click&vd_source=ac8b640f4a150539508492987a2efe62),配置基本参数
* 在Vscode中添加插件Debugger for unity(已弃用,点击量最高的)
* 参照[这个](https://blog.csdn.net/nick1992111/article/details/128580845?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522168544015616800215098367%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=168544015616800215098367&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-2-128580845-null-null.142^v88^koosearch_v1,239^v2^insert_chatgpt&utm_term=debugger%20for%20unity%E5%B7%B2%E5%BC%83%E7%94%A8&spm=1018.2226.3001.4187),完成最后调试

#### 单帧调试

​	在脚本中不设置断点,点击Unity运行;在需要调试的时机暂停游戏,在脚本中增加断点;最后在Unity中逐帧运行,在脚本中调试

### 使用MonoDevelop

* 加断点
* 启动调试:点击MD菜单栏"Run"->Attach to Process
* 在Unity中Play场景

# Unity编程

## 特性

> [serializeField] : 在编辑器中显示私有变量
>
> [HideInInspector] : 在编译器中隐藏字段
>
> [Range(min,max)] : 在编译器中限制范围

## 注意事项:

​	在**Unity脚本**中,通常不写属性,也因此Unity界面中也不显示属性,只显示字段.在脚本中也**不写构造函数,**因为不能在子线程中访问主线程的成员.

​	Assembly-CSharp.dll保存脚本

# 常用API

## Component

​	提供关于组件的功能.(通常从自身开始查找).

​	Base class for everything attached to a [GameObject](https://docs.unity3d.com/ScriptReference/GameObject.html).Note that your code will never directly create a Component. Instead, you write script code, and attach the script to a [GameObject](https://docs.unity3d.com/ScriptReference/GameObject.html). See Also: [ScriptableObject](https://docs.unity3d.com/ScriptReference/ScriptableObject.html) as a way to create scripts that do not attach to any [GameObject](https://docs.unity3d.com/ScriptReference/GameObject.html).

​	Note that your code will never directly create a Component. Instead, you write script code, and attach the script to a [GameObject](https://docs.unity3d.com/2023.2/Documentation/ScriptReference/GameObject.html). 



​	**gameObject**:The game object this component is attached to. A component is always attached to a game object.

​	**GetComponent**:获取指定组件

​	**GetComponents**:获取指定对象的全部组件

​	**GetComponentsInChildren**:获取自身及所有子代物体的指定组件

​	**GetComponentInChildren**:获取自身或子代物体的指定物件

​	**GetComponentsInParents**:获取自身及所有父代物体的指定组件

​	**GetComponentInParent**:获取自身或父代物体的指定组件

​	**CompareTag**:比较指定组件的Tag与待比较的字符串是否相同

## Transform

​	提供了关于组件参数等功能.

​	对象的位置、旋转和缩放。

​	场景中的每个对象都有一个变换。 它用于存储和操作对象的位置、旋转和缩放。 每个变换都可以有一个父级，让您能够分层应用位置、旋转和缩放。这是“Hierarchy”面板中显示的层级视图。



​	**position**:世界空间中的变换位置.

​	**localPosition**:Position of the transform relative to the parent transform..

​	[**lossyScale**](https://docs.unity3d.com/2020.2/Documentation/ScriptReference/Transform-lossyScale.html):The global scale of the object (Read Only).

​	**translate**:Moves the transform in the direction and distance of translation.

​	RotateAround:Rotates the transform about `axis` passing through `point` in world coordinates by `angle` degrees.

​	**root**:Returns the topmost transform in the hierarchy.

​	SetParent:This method is the same as the [parent](https://docs.unity3d.com/ScriptReference/Transform-parent.html) property except that it also lets the [Transform](https://docs.unity3d.com/ScriptReference/Transform.html) keep its local orientation rather than its global orientation. This means for example, if the GameObject was previously next to its parent, setting `worldPositionStays` to false will move the GameObject to be positioned next to its new parent in the same way.The default value of `worldPositionStays` argument is true.

​	#If you use the SetParent, you need to set a public Transform, then drag a object  to the object with scripts.

​	**parent**:Get the parent object.

​	**Find**:Finds a child by name `n` and returns it.

​	**getchild**:Returns a transform child by index.

​	**LookAt**:Rotates the transform so the forward vector points at /target/'s current position.

​	**SetsiblingIndex**:Use this to change the sibling index of the GameObject. If a GameObject shares a parent with other GameObjects and are on the same level (i.e. they share the same direct parent), these GameObjects are known as siblings. The sibling index shows where each GameObject sits in this sibling hierarchy.

## GameObject

​	提供关于对象的一些操作

​	Base class for all entities in Unity Scenes. All object in hierarchy is gameobject



​	**activeSelf**:The local active state of this GameObject. (Read Only)

​	**activeInHierarchy**:Defines whether the GameObject is active in the Scene.This lets you know whether a GameObject is active in the game. That is the case if its [GameObject.activeSelf](https://docs.unity3d.com/ScriptReference/GameObject-activeSelf.html) property is enabled, as well as that of all its parents.

​	**SetActive**:Activates/Deactivates the GameObject, depending on the given `true` or `false` value.

​	**AddComponent**:Adds a component class of type `componentType` to the GameObject. C# Users can use a generic version.

## Object

​	Object is the base class of all built-in Unity objects. Custom Unity Object types can be defined in scripts by deriving a new class from types like [MonoBehaviour](https://docs.unity3d.com/2023.2/Documentation/ScriptReference/MonoBehaviour.html), [ScriptableObject](https://docs.unity3d.com/2023.2/Documentation/ScriptReference/ScriptableObject.html) and [ScriptedImporter](https://docs.unity3d.com/2023.2/Documentation/ScriptReference/AssetImporters.ScriptedImporter.html).



​	**Instantiate**:Clones the object `original` and returns the clone.

​	**Destroy**:Removes a GameObject, component or asset.

​	**FindObjectOfType**:Returns the first active loaded object of Type `type`.

## Time

The interface to get time information from Unity.

**deltaTime**:The completion time in seconds since the last frame (Read Only).通过这个数据可以控制在不同帧率下的恒定时长:

```C#
this.Transform.Rotate(0,1*Time.deltaTime,0);//恒定转速
```

**timeScale**:The scale at which time passes. This can be used for slow motion effects.若写在Update之外则不会影响到Update,因为Update受渲染的影响,而渲染与机器性能有关,与timescale无关.但是timescale可以通过控制deltatime来影响update内的物体.同时timescale可以影响fixupdate,因为fixupdate是物理更新,而不是渲染更新.

```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

/// <summary>
///We compare the different between deltaTime and unscaledDeltaTime
/// in timeScale
/// </summary>

public class TimeScale : MonoBehaviour
{
    [Range(0,10)]
    public int speed=2;
    public void Update(){
        this.transform.Rotate(0,speed*Time.unscaledDeltaTime,0);
    }
	private void OnGUI(){
        if(GUILayout.Button("Stop")){
            Time.timeScale=0;
        }
        if(GUILayout.Button("Start")){
            Time.timeScale=1;
        }
    }
}
```

​	**time**:The time at the beginning of this frame (Read Only). This is the time in seconds since the start of the game.

​	

# 练习

## 查找血量最低的敌人

```C#
//HP
using UnityEngine;
using System.Collections;

public class Enermy : MonoBehaviour{
    public int HP;
}
/*******************************************/
//Find the lowest HP enermy
using UnityEngine;
using System.Collections;

public class FindLowestHP : MonoBehaviour{
    private void OnGUI(){
        if(GUILayout.Button("Find the enermy with min HP")){
            Enermy[] allEnermy = Object.FindObjectOfType<Enermy>();
            Enermy min = FindEnermyByMinHP(allEnermy);//Find the enermy
            min.GetComponent<MeshRenderer>().material.color=blue;//Change the special enermy's material
        }
    }
    
    public Enermy FindEnermyByMinHP(Enermy[] allEnermy){
        Enermy min = allEnermy[0];
        for(int i=0;i<allEnermy.Length;i++){
            if(min.HP > allEnermy[i].HP){
                min = allEnermy[i];
            }
        }
        return min;
    }
}
```

## 查找未知层级的物体

```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

/// <summary>
///This script is a assist to TransForm
/// </summary>

public class TranFormHelper
{
    //Find the child in unkown layer
    //parentTF:Parent object transform component
    //ChildName:Child object name
    public static Transform GetChildRecursion(Transform parentTF, string ChildName){
        Transform childTF = parentTF.Find(ChildName);
        if(childTF != null){
            return childTF;
        }
        int count = parentTF.childCount;
        for(int i=0; i < count; i++){
            childTF = GetChildRecursion(parentTF.GetChild(i),ChildName);
            if(childTF != null){
                return childTF;
            }
        }
        return null;
    }
    
}
```

## 查找举例最近的目标

```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

/// <summary>
/// Find the min distance in targets to the object with this script.
/// </summary>

public class ComponentDemo : MonoBehaviour
{
    private void OnGUI(){
        if(GUILayout.Button("FindMinDistance")){
            Target[] targets = Object.FindObjectsOfType<Target>();
            Target min = FindMinDistance(targets);
            min.GetComponent<MeshRenderer>().material.color=Color.blue;
        }
    }
    public Target FindMinDistance(Target[] targets){
        Target min=targets[0];
        int count = targets.Length,enter=0;
        float distance = 0, finaldistance=0;
        for(int i=0;i<count;i++){
            if(targets[i].gameObject!=this.gameObject){
                distance = Vector3.Distance(targets[i].gameObject.transform.position,this.transform.position);
                if(enter==0){
                    finaldistance = distance;
                    enter=1;
                    min=targets[i];
                }else{
                    if(distance < finaldistance){
                        finaldistance = distance;
                        min=targets[i];
                    }
                }
            }
        }
        return min;
    }
}
```

## 倒计时

```C#
using System.Reflection.Emit;
using System.Collections;
using System.Collections.Generic;
using TMPro;
using UnityEngine;

/// <summary>
///count down
/// </summary>

public class TextDemo : MonoBehaviour
{
    //Find the component
    [Range(0,300)]
    public int second=120;
    private TextMeshProUGUI textTimer;
    private void Start(){
        textTimer = this.GetComponent<TextMeshProUGUI>();
        InvokeRepeating("UpdateTime3",1,1);
    }

    //In this way, every time you render, you have to decide if the conditions are right.
    private float delta=1;
    private void UpdateTime1(){
        if(Time.time>delta){
            second--;
            textTimer.text=$"{second/60:d2}:{second%60:d2}";
            delta++;
        }
    }

    //This is determined by accumulating the render time per frame.
    private float sum=0;
    private void UpdateTime2(){
        sum+=Time.deltaTime;
        if(sum>=1){
            second--;
            textTimer.text=$"{second/60:d2}:{second%60:d2}";
            sum=0;
        }
    }
    //In this way, you need to use the "InvokeRepeating" in Start().
    private void UpdateTime3(){
        if(second <= 1){
            CancelInvoke("UpdateTime3");
        }
        second--;
        textTimer.text=$"{second/60:d2}:{second%60:d2}";
    }
}
```

# Hierarchy面板

## 预制件Prefab

​	预制件是一种资源类型,可以多次在场景进行实例.

​	优点:对预制件的修改,可以同步到所有实例,从而提高开发效率.如果单独修改实例的属性值,则该值不再随预制件变化,直至按下Apply按键.

​	Select:通过预制件实例选择对应预制件

​	Revert:放弃实例属性值,还原预制件属性值

​	Apply:将某一实例的修改应用到所有实例



# Animation View

​	通过动画视图可以直接创建和修改动画片段(Animation Clips).显示动画视图:Window-Animation

## 创建动画

* 为物体添加Animation组件
* 在Aniamtion View中创建片段
* 在Hierarchy面板中点击要创建Animation的物体
* 在Aniamtion面板中Add Property



## 函数

* IsPlaying	   :Is the animation named name playing?
* Play               :Plays an animation without blending.
* PlayQueued:Plays an animation after previous animations has finished playing.
* Rewind         :Rewinds the animation named name.
* CrossFade   :Fades the animation with name animation in over a period of time seconds and fades other animations out.

```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

/// <summary>
///
/// </summary>

public class DoorAction : MonoBehaviour
{
    //We use the OnMouseDown to use the animation(the precondition is the collider)
    public bool doorStation = false;
    private string animationName = "Door";
    private Animation animationdoor;
    private void Start()
    {
        animationdoor = GetComponent<Animation>();
    }
    private void OnMouseDown()
    {
        if (!doorStation)
        {
            animationdoor[animationName].speed = 1;//Open the door
        }
        else
        {
            animationdoor[animationName].speed = -1;//Close the door
            if (animationdoor.isPlaying == false)
                animationdoor[animationName].time = animationdoor[animationName].length;//Make sure that the initial state is in the end
        }
        animationdoor.Play(animationName);
        doorStation = !doorStation;
    }
}
```

# 实例项目

## 敌人模块

### 需求

* 敌人沿指定路线运动
* 受到攻击后降低生命值,生命值归零死亡
* 运动播放跑步动画,攻击播放攻击动画,攻击间隔播放闲置动画,死亡播放死亡动画

