#Unity UI

## 简介：
Unity UI创建新项目的时候建议把主摄像机，背景，粒子系统等，用一个空对象装在一起拖到Projectd的Prefabs里,方便复用。UI界面需要各种与背景不同步的效果时，需要一个GUI摄像机。
UI所有的内容都需要放置到canvas上面，可以创建多个canvas，不过一般情况下，一个就够了。如果只是需要不受canvas设置影响的新UI界面，可以把需要特殊Cavans设置的内容放入windows（空对象创建），来装载动画脚本等内容。
这里的所有案例来自商店 **Unity Samples: UI**（由于Unity版本更新,案例里面有的写法不对,尤其是4个状态的按键动画,仿照不能完成的内容,文档均有提及）
 
##1.	Canvas（画布）
 ![canvas][1]
Canvas group可以调节canvas所有元素的这三个值，避免每个组件勾选。
 ![Scale][2]
Canvas Scale用于调节UI组件对于屏幕的响应。
UI Scale Mode上表三个属性，第一个使UI组件像素大小固定，第二个响应屏幕尺寸，第三个是固定物理尺寸。这里面只讲下响应屏幕尺寸的具体设置，如下图。

- 第一个是UI设计时的分辨率 
![a][3]
- 第二个是如何测量屏幕大小
![b][4]
- 第三个是匹配屏幕宽高比例 
![c][5]

![d][6]
## 2.	Rect Transform（矩形变换）
![rec][7]  
Pos X Y Z 定义矩形相当于锚的轴心点的位置，如图Pos Y是轴心距离父级元素上边界的距离。
Width/Height是宽高，也是根据轴心点扩展的。
Left right top bottom, 定义了矩形边缘相对于锚点的位置，如图为左侧和右侧距离父级元素左右边界的距离。
Anchors就是锚点，既上面的小花瓣，默认是一个，可以扩展成两个和四个，从而更方便的定位子元素，并且适应不同屏幕大小的变化跟随父元素变换。Min和Max分别是矩形左下和右上的锚点，点击上面的stretch可以直接选择锚点分布，会自动填写这个。
Pivot定义矩阵旋转的中心点坐标，默认都是0.5 0.5，一定要注意它在屏幕上是一个圆圈的小点，可以拉动到物体外面，进而影响很多值。
Rotation定义围绕中心点的旋转角度，在UI里面一般只更改z轴。
Scale定义对象的缩放系数，一般不要使用，因为会缩放包括字体子元素在内所有元素。
个人理解：GUI里面的位置和前端里面绝对相对位置类似。如果把四个锚点放在父元素的四个角上，就可以通过Left right top bottom来控制元素的位置和大小，父元素变换的时候子元素大小与位置跟随父元素一同变换。如果把锚点放在父元素的一个角，父元素变换时相对于这个角的距离百分比不改变，子元素大小并不跟随父元素变换。锚固定为两个点的时候，子元素只有一个方向的大小是跟随变换的，位置相当于横纵轴等百分比变化。
此外建议吧元素中心点（pivot）调节到锚点的同一个角，这样写位置的时候更清晰。
如图，锚点和中心点都在左下角，就可以看到距离屏幕左下的距离。右图是不调节。
![left][8]  ![left][9] 
##3.	Button（按键）
 ![5.png](https://i.loli.net/2017/12/12/5a2f7f1930b8a.png)
Interactable一般选中，代表该组件接受输入。Transtiton就是划过选中的变化，可以改变你颜色，也可以加入动画，Color Multiplier可以调节所有颜色的亮度，Fade Duration就是颜色改变的事件（过渡时间）。
Button这里介绍两个关闭的绑定事件，GameObject.SetActive(关闭按钮），ActiveStateToggler.ToggleActive(开关控制）

##4.	Event Trigger/Event System（事件触发器/事件系统）
 ![6.png](https://i.loli.net/2017/12/12/5a2f7f1b764b3.png)
大多数object都可以绑定事件，和DOM事件一样，可以选择包括鼠标键盘监听等一系列的事件直接操作。前面Runtime Only下面对应的是要触发的对象而不是绑定事件的对象，选择改变的具体时间后可以输入值。
Event System在创建UI元素事的时候会自动创建，是GUI系统自带的事件管理系统，如果不小心删除或者项目没有带，可以自己在Hterarchy创建。
##5.	Mask / Scroll Rect（遮罩/滚动区域）
 ![7.png](https://i.loli.net/2017/12/12/5a2f7f271d849.png)
  遮罩要写在父级元素里面，用于遮罩子元素。除了要添加mask组件外，还要添加image，作为遮罩材料。Image不需要选择，就会根据父元素形状裁剪，如果选择了就会裁剪成image遮罩的形状。
  Scroll Rect同样放在父级元素上，添加后可以自由拖动查看子元素图片。Content就是你要观看的内容背景。H/V还是横列纵列的意思。Movement type是选择移动方式，默认的elastic可以移动出背景图漏出主背景图，并有一个动画的弹性返回。Unrestricted拖到哪就是哪，clamped限制在子元素图片内拖动。
  加上inertia之后选择默认移动方式，拉出子元素图片弹回的时候有个动画的延迟，比较自然，下面可以调节速度。Scroll就是针对鼠标滚轮的敏感度了。下面可以添加scrollbar组
件用来拉动。

##6.	Slider（滑动条）
![8.png](https://i.loli.net/2017/12/12/5a2f7f27d9939.png)   
Interactable如上所述代表可交互的，不勾选则会禁用slider。
Slider的值在min value和max value之间变动，下面的value是初始的值，勾选whole value将只能在整数区间变换。
Transition选择 color int上面所有颜色改变都是对于滑块(你拖动的那个圆圈)的各个状态,也可以选择没有变化或者动画效果。
没有划过的区域颜色在Backgroud里面改变，划过的在fill里面，handle（滑块）的改变在Handle Slide Area里面。
Direction允许你选择自己喜欢的滑动方向。
  ![9.png](https://i.loli.net/2017/12/12/5a2f7f1c7acf7.png)
可以给滑动条添加事件监听（值变换），绑定事件摄像机视野的话，可以通过滑动滑块改变背景图片距离（注意最大值调整，太小了看不到全景）。也可以绑定光线，调节亮度或者颜色，如：ChangeColor.SetRed，或者显示百分比，绑定标签lable：ShowSliderValue.UpdateLabel。
注意：
如果使用C#控制滑动,把Interactable关上，避免冲突。

##7.	Scroll bar（封装好的滑动工具）
Scroll Bar和上面的slider相似的内容这里不再赘述。
滚动内容可以通过mask和scroll rect来实现
![10.png](https://i.loli.net/2017/12/12/5a2f7f2891ade.png)
如果播放的时候滚动条消失或者出现在奇怪的位置上，基本上由于调整位置的时候出错，删掉了重新更改位置就可以解决问题。{一般使用Unity新带的自动布局（包括vertical group /horizontal group）容易出现，很简单的UI尽量自己去调整吧，官网对这个自动布局系统介绍的很简洁，出了问题也不知道是什么}

##8.	Drop Down（下拉菜单）
![11.png](https://i.loli.net/2017/12/12/5a2f7f1d811bc.png)![12.png](https://i.loli.net/2017/12/12/5a2f7f1cce24d.png)
 
第一个选项放在label里面，第二个放在Item下面的Item Label里面，都拉入上图的Text里面，Options改成对应的文字。增加选项只需要添加Option就可以了，菜单过长的时候使用搭配的scroll bar组件。

##9.	Raw Image/Image（原始图片/图片）
图片组件传入图片在source image上，材料在Material添加。图片组件可以作为拖拽区域覆盖在原来的UI上，需要透明只需要把Material换成天空盒子。
未加工的图片，只为我们提供了修改UV的方法，除此之外都是继承自MaskableGraphic的方法。一般情况下使用image而不是使用它。这里举例一个用法：当需要使用RenderTexture透视背景的时候，使用Raw Image组件
![14.png](https://i.loli.net/2017/12/13/5a30ce4f765ec.png)![15.png](https://i.loli.net/2017/12/13/5a30cf55d0d49.png)
如图，rawimage绑定了渲染贴图（rendertexture),可以通过额外的摄像机完成对背景图的透视。
渲染贴图可以在Project创建，贴图内容为绑定摄像机的场景。
![15.png](https://i.loli.net/2017/12/13/5a30ce63c8acf.png) 

##10.UI动画系统（官网案例状态机错误）
![20.png](https://i.loli.net/2017/12/13/5a30f5fab2ef8.png)
实现如图所示的动画：鼠标滑过每个字母块的时候字母块浮动，点击时固定浮动效果。（动画效果片段是导入的）
首先在Project创建Animatior Controller控件，双击打开，在Animator进行动画状态机编写。
![21.png](https://i.loli.net/2017/12/13/5a30f5eb0e38d.png)
如图所示，不需要修改默认的layers,只需要在Parameters(参数)里面增加四个状态的Trigger，拉入动画片段，绘制如图状态机。然后点击Anystate,分别编辑每个状态。
![22.png](https://i.loli.net/2017/12/13/5a30f80f0af8c.png)
如图，在每个Condition加入自己的触发器，再调整动画时间等具体的参数就可了。
这里Preview Source state恒定指向entry进入后的默认状态，无法更改。
注：这个键盘，使用Grid写的，只需要打开一个空对象，加入Grid Layout Group & Layout Element组件，他的按键子元素会自动排版在设定区域。记住写完一个带好动画的子元素后拖到Prefabs保存，就可以批量制造了。

##11.UI摄像机/光源
![19.png](https://i.loli.net/2017/12/13/5a30ead478dac.png)

- 图片上最左侧的摄像机是GUI摄像机，中间的是主摄像机（Main Camera），右侧的是RTCamera，这个顺序和位置不重要，保证视图效果就可以了。相机重要的顺序参数是Depth，主摄像机一般设置为-1,UI层是0,依次叠加渲染顺序。图片左右侧图片是背景图，UI层背景图来自透视纹理与RT摄像机的共同作用。
- 其中主摄像机渲染的是背景图，GUI摄像机只渲染UI层，RT相机渲染绑定了透视纹理的UIraw image以及飞行器模型。在Unity里面最终成像是各个摄像机共同渲染的结果，每个摄像机只渲染culling musk（选择遮罩）选定的层，没背渲染的层就是空白。一般UI场景都有两个摄像机，主摄像机渲染背景，GUI摄像机渲染UI层。这样UI层可以独立于背景移动，有很好的立体效果（比如示例效果，使UI层跟随鼠标倾斜交互，就是通过脚本控制canvas--windows的local rotation来实现的，这样复杂的场景就无法通过单一摄像机实现）。
- 相机选项Projection（投射）有两个选项，Perspective表示远景，是摄像头开始向画面扩散渲染，和美术的透视图近大远小是一个原理，投射出来的场景就是立体的。Orthographic是平行投射，从无线远处平行投射渲染，得到的是立体图形在平面上的投射，是2D的渲染效果。
- 这个场景的RT摄像机是为了渲染UI层上面的飞行器模型以及绑定透视纹理。他不渲染UI层，因此视图只有背景和机器人，投射到透视纹理区域显示就是最终的页面效果。
- 图片下方光源是蓝色点光源，上方是白色点光源，都是通过设置选择遮罩，只照射UI层来创造漂亮的打光效果。小太阳是Directional light（定向光源），用来点亮飞行器模型，因为这个场景没有其他定向光源。注意，场景背景图是不需要光线来渲染的，其他物体都是需要的。





  [1]: https://i.loli.net/2017/12/12/5a2f7b1a056f7.png
  [2]: https://i.loli.net/2017/12/12/5a2f7b1b5933f.png
  [3]: https://i.loli.net/2017/12/12/5a2f7cb8ebf87.png
  [4]: https://i.loli.net/2017/12/12/5a2f7cb8ed700.png
  [5]: https://i.loli.net/2017/12/12/5a2f7cb935be2.png
  [6]: https://i.loli.net/2017/12/12/5a2f7d983cb31.png
  [7]: https://i.loli.net/2017/12/12/5a2f7d98b0856.png
  [8]: https://i.loli.net/2017/12/12/5a2f7d98e54cc.png
  [9]: https://i.loli.net/2017/12/12/5a2f7d9925c41.png