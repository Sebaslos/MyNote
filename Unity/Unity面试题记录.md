# Unity 面试题记录

1. 什么是协同程序？

    - 在主线程运行时同时开启另一段逻辑处理，来协助当前程序的执行。协程是一个分部执行，遇到条件（yield return 语句）会挂起，直到条件满足才会被唤醒继续执行后面的代码
    - Unity在每一帧（Frame）都会去处理对象上的协程。Unity主要是在Update后去处理协程（检查协程的条件是否满足）
    - 启动协程时，协程会立即运行到第一个 yield return 语句处，如果是 yield return null ，就会在同一帧再次被唤醒
    - 具体见[Unity生命周期流程图](https://docs.unity3d.com/Manual/ExecutionOrder.html)

1. Unity3D多线程程序

    - 可以用thread创建多线程
    - 但是仅能从主线程中访问Unity3D的组件，对象和Unity3D系统调用
    - 可以使用lock关键字,以确保只有一个线程可以在特定时间内访问特定的对象

1. Prefab的作用

    - 在游戏运行时实例化，prefab相当于一个模板，对你已经有的素材、脚本、参数做一个默认的配置，以便于以后的修改

1. 如何安全的在不同工程间安全地迁移asset数据

    - 将Assets目录和Library目录一起迁移
    - 导出包，export Package
    - 用unity自带的assets Server功能

1. Unity3d提供了几种光源，分别是什么

    - 平行光：Directional Light
    - 聚光灯：Spot Light
    - 点光源：Point Light
    - 区域光源：Area Light（只用于烘培）

1. unity3d从唤醒到销毁有一段生命周期，请列出系统自己调用的几个重要方法

    Reset -> Awake –> OnEnable –> Start –> FixedUpdate –> Update –> LateUpdate –> OnGUI –> OnDisable –> OnDestroy

1. 物理更新一般在哪个系统函数里

    - FixedUpdate，是在固定的时间间隔执行，不受游戏帧率的影响
    - 处理Rigidbody的时候最好用FixedUpdate
    - FixedUpdate的时间间隔可以在项目设置中更改，Edit->ProjectSetting->time  找到Fixedtimestep

1. 移动相机动作在哪个函数里，为什么在这个函数里

    - LateUpdate，是在所有update结束后才调，比较适合用于命令脚本的执行
    - 官网上例子是摄像机的跟随，都是在所有update操作完才跟进摄像机，不然就有可能出现摄像机已经推进了，但是视角里还未有角色的空帧出现

1. 简述一下对象池

    频繁创建和销毁游戏对象会为脚本执行和垃圾回收带来很大的性能开销
    - 创建一个数组或链表，作为对象池。实例一组对象，设置为未激活状态，放入对象池中
    - 创建对象时从对象池中取出一个未激活对象，激活该对象，设置相关属性，然后直接使用
    - 销毁对象时禁用该对象，然后放回对象池即可

1. 一个场景放置多个camera并同时处于活动状态，会发生什么

    - 实际看到的画面由多个camera的画面组成，由depth、Clear Flag、Culling Mask都会影响最终合成效果

1. 简述四元数的作用，四元数对欧拉角的优点

    四元数用于表示旋转\
    相对欧拉角的优点:
    - 能进行增量旋转
    - 避免万向锁
    - 给定方位的表达方式有两种，互为负（欧拉角有无数种表达方式）

1. 向量的点乘、叉乘以及归一化的意义

    - 点乘描述了两个向量的相似程度，结果越大两向量越相似，还可表示投影
    - 叉乘得到的向量垂直于原来的两个向量
    - 标准化向量：用在只关心方向，不关心大小的时候

1. 为何大家都在移动设备上寻求U3D原生GUI的替代方案

    不美观，OnGUI很耗费时间，使用不方便 ，DrawCall

1. 请简述如何在不同分辨率下保持UI的一致性

    - 使用NGUI，屏幕分辨率的自适应性
    - UGUI通过锚点和中心点和分辨率

1. 请简述NGUI中Panel和Anchor的作用？？？

    - Panel是一个容器，它将包含所有UI小部件，并负责将所包含的部件组合优化，以减少绘制命令的调用
    - Anchor是NGUI中屏幕分辨率的自适应性，来适应不同的分辨率的屏幕显示

1. 使用Unity3d实现2d游戏，有几种方式

    - 使用本身的GUI
    - 把摄像机的Projection(投影)值调为Orthographic(正交投影)，不考虑z轴
    - 使用2d插件，如：NGUI、2DToolKit

1. Unity3d中的碰撞器和触发器的区别

    - 碰撞器是触发器的载体，而触发器只是碰撞器身上的一个属性
    - 碰撞器（Collider）有碰撞效果，IsTrigger=false，可以调用OnCollisionEnter/Stay/Exit函数
    - 触发器(Trigger)没有碰撞效果，isTrigger=true，可以调用OnTriggerEnter/Stay/Exit函数

1. 物体发生碰撞的必要条件

    两个物体都必须带有碰撞器(Collider)，运动的物体还必须带有Rigidbody刚体。

1. CharacterController和Rigidbody的区别

    Rigidbody具有完全真实物理的特性，而CharacterController可以说是受限的的Rigidbody，具有一定的物理效果但不是完全真实的

1. u3d中，几种施加力的方式

    rigidbody.AddForce/AddForceAtPosition

1. MeshCollider和其他Collider的一个主要不同点

    MeshCollider是网格碰撞器，对于复杂网状模型上的碰撞检测，比其他的碰撞检测精确的多，但是相对其他的碰撞检测计算也增多

1. MeshRender中material和sharedmaterial的区别

    修改sharedMaterial将改变所有物体使用这个材质的外观，并且也改变储存在工程里的材质设置

1. LOD是什么，优缺点是什么

    LOD技术 (Level of Details)：为同一个物体准备多个精度不同的模型，如果物体距离摄像机较近，使用高精度模型绘制物体，展现细节。如果较远，使用低精度模型绘制物体，提高渲染效率
    - 优点：可根据距离动态地选择渲染不同细节的模型
    - 缺点：加重美工的负担，要准备不同细节的同一模型，同样的会稍微增加游戏的容量

1. MipMap是什么、作用

    Mipmap是一种纹理技术，是目前解决纹理分辨率与视点距离关系的最有效途径。
    - 提升渲染性能，还能减少远处因为分辨率较大的纹理因过分缩小而产生的失真
    - 提高渲染速度和渲染效果
    - 增加内存消耗

1. 什么是LightMap

    就是指在三维软件里实现打好光，然后渲染把场景各表面的光照输出到贴图上，最后又通过引擎贴到场景上，这样就使物体有了光照的感觉
    - 使用光照贴图比使用实时光源渲染要快
    - 可以降低游戏内存消耗
    - 多个物体可以使用同一张光照贴图

1. 动态加载资源的方式

    - Resources.Load()
    - AssetBundle
    - Resources资源的加载是动态加载内部的资源(位于Resources目录下)
    - AssetBundle 是动态加载外部的资源(.assetbundle文件)

1. 什么叫做链条关节

    Hinge Joint，可以模拟两个物体间用一根链条连接在一起的情况，能保持两个物体在一个固定距离内部相互移动而不产生作用力，但是达到固定距离后就会产生拉力

1. Material和Physic Material区别

    - PhysicMaterial 物理材质：主要是控制物体的摩擦，弹力等物理属性
    - Material 材质：主要是控制一个物体的颜色，质感等显示

1. 写出Animation的五个方法

    AddClip 添加剪辑、Blend 混合、Play 播放、Stop 停止、Sample 采样 、CrossFade淡入淡出切换动画、IsPlaying是否正在播放某个动画

1. 请描述游戏动画有哪几种，以及其原理

    主要有关节动画、单一网格模型动画(关键帧动画)、骨骼动画
    - 关节动画：把角色分成若干独立部分，一个部分对应一个网格模型，部分的动画连接成一个整体的动画，角色比较灵活，Quake2中使用这种动画
    - 单一网格模型动画由一个完整的网格模型构成，在动画序列的关键帧里记录各个顶点的原位置及其改变量，然后插值运算实现动画效果，角色动画较真实
    - 骨骼动画，广泛应用的动画方式，集成了以上两个方式的优点，骨骼按角色特点组成一定的层次结构，有关节相连，可做相对运动，皮肤作为单一网格蒙在骨骼之外，决定角色的外观，皮肤网格每一个顶点都会受到骨骼的影响，从而实现完美的动画

1. 如何优化内存

    - 压缩自带类库
    - 将暂时不用的以后还需要使用的物体隐藏起来而不是直接Destroy掉
    - 释放AssetBundle占用的资源
    - 降低模型的片面数，降低模型的骨骼数量，降低贴图的大小
    - 使用光照贴图

    - 降低资源的大小
      - 导入时压缩资源
      - 使用压缩纹理：如为安卓平台的纹理设置ETC1纹理压缩格式
      - 使用压缩的音频：使用MP3保存背景音乐，使用WAV保存音效
      - 调整光照贴图的大小
    - 及时释放不用的资源
      - 及时调用Destory函数
      - 定期调用GC.Collect函数，手动进行垃圾回收

1. 着色器

    - 着色器本质上是一种运行在显卡GPU上的程序，用于控制显卡的图形渲染过程
    - 着色器语言：HLSL，GLSL，Cg
    - Unity中所有的渲染都需要通过Shader来完成
    - Unity使用自定义ShaderLab开发语言组织着色器