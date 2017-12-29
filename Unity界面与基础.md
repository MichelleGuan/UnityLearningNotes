# Unity界面与基础
## 简介：
  打开Unity后，首先调节右侧layout,到如下布局模式。
![布局][1]
本文档和接下来的所有Unity文档都是根据Unity圣典的补充，清先通过看书对Unity有一个初步的了解。教程的所以内容出自Unity官网视频和文档，清点击以下链接查看。

- [官方英文文档](https://docs.unity3d.com/Manual/LearningtheInterface.html?_ga=2.234943811.393962825.1512458505-1303291460.1512458505)
- [C# scripting API](https://docs.unity3d.com/540/Documentation/ScriptReference/Networking.NetworkSystem.ErrorMessage.html)
- [官方英文视频教程，需要梯子](https://unity3d.com/cn/learn/tutorials)
- [Unity圣典中文版](http://www.ceeger.com/Manual/)
- [腾讯Unity官方英文视频，不需要梯子](http://v.qq.com/vplus/1e710e7fb0638396abe3c6c6aff3832c)

## 基本概念：
1.	Unity3D坐标系的方向是从2D扩展的，因此不是常用的坐标系。XY与2D模式一样，Z轴向屏幕内侧扩展。
2.	左侧是global coordinate system（数学坐标系）
 右侧是local coordinate system（轴心坐标系）
![坐标系][2]
Unity每个对象都有自己的轴心，组合也有轴心。可以选择绝对位置和相对位置。因此同一个物体的位置有多种表示方式，在下一篇UI文档中有详细介绍。
 
3.	文件系统（Hierarchy）和Dom树类似，也是采用父子节点的模式,改变父节点的内容会影响到子节点，在操作时可以选择父节点，调整所有属于这一节点的内容。
元素显示层级和文件系统的顺序是对应的，想要把内容上移一层只需要在文件系统插入到前面。
Hierarchy feels like a DOM tree; you can control children elements by select parent node and transform it.

## 界面内容和快捷键：
  在Unity中，大多数的3D移动可以通过按住鼠标右键WASD来完成。再具体移动一个物体或者调整其大小的时候。尽量按照XYZ方向改变，最后调整角度。任意拖动后再恢复位置就会变得麻烦，改变角度后XYZ方向都会改变，不好移动位置。任何错误操作都可以通过ctrl+z来撤回，调试尽量使用播放模式，这样结束播放的时候，你所有操作都会归零。最后，具体调试数值的时候没必要一个个输入，鼠标在数值名字上按住左右滑动就可以改变数字。
1.	界面右上方的操作工具（拖拽 移动 角度 大小 2D缩放）  对应快捷键QWERT
The scene view tool bar. Corresponding keys: Q W E R T
 ![快捷键][3]
First four icons are easy to understand, first is hand tool, second is translate tool, third is rotate tool, fourth tool is scale tool.
第四个快捷键一般只适用2D和UI场景，同时变换角度和大小。
Fifth tool is transform tool, which is designed to manipulate and size 2D objects, it can translate rotate and scale a game object at the same time. Although it is functional on 3D objects, it’s better to use on 2D or UI objects.
2.	按住右键点击WASD可以移动到任何你想移动的地方，使用QE调节高度。
Holding on right mouse button can go to flight mode with you can view around by press WASD, and QE to change your height.
3.	Alt+鼠标左键更改角度，Alt+鼠标右键调节远近。
ALT+LEFT mouse button, change rotates anytime. ALT+RIGHT mouse button, drag to zoom in and out.
4.![轴心][4]	   
Pivot mode 轴心模式（以选择目标作为轴心旋转，调节位置） center mode 中心模式（以组合轴心作为目标旋转，调节位置）  3D多对象时差异明显，2D没什么差别。
5.	F frame selected. (鼠标必须放在scene view上)，也可以在Hierarchy（层级目录）双击目标实现。
6.	Scene菜单从左到右分别是： Draw mode menu（层级图等多种视图） 2D/3D切换 图片icon用于单独开启或禁用元素。   
![菜单][5]
7.	 Gizmos选择具体显示元素的下拉菜单，也可以调节图标（摄像机，太阳）大小。
8.	 Gizmos右边搜索框除了搜索的内容，其他元素变灰色
9.	播放时可以通过暂停来测试效果，播放过程中的所有改变都不会被保存。
10.	调整Project层级关系需要在Unity进行，不能直接在文件夹编辑。可以在Project的搜索里面通过保存搜索结果建立favorite folder,通过拖拽就可以加元素。
11.	 Inspector第一个组件，对象（具体功能如下，对应图片前两行）
选择图标icon  勾选是不是active  名字  勾选参与数据运算  Tag(script)操作  layer设置（具体属于哪一层）
![Inspector][6]
第二部分Transform （数字的值可以直接在名字上面滑动调节）
可以直接调节父级元素的属性值，来调节所有子元素。可以锁定Inspector，也可以打开几个。
选择Debug模式，会显示更多属性和值，用来找问题。大多数属性都可以禁用，可以复制到其他的Object下面，或者拖拽其他地方。
直接把Script等Asset拖拽到Object上可以自动生成这个组件.
你在layer里面选的种类可以在下面的组件里面定制选择。比如Add layer加一个ignore camera,在相机选项里面不显示这个layer的内容（projection），可以忽略光线等。
12.	File--Build setting选择平台和平台设置（关于如何生成exe的可运行文件）
13.	Window—Profiler（性能检测） 
![性能][7]
14.	在游戏里面创建或者需要调整的Object尽量放到Project面板，在里面编辑后可以应用Apply和revert去更改所有元素或者撤销更改。也可以使用万能CTRL Z方法撤销。
15.	更改Scale和rotate会影响position和transform，在调整的时候注意逻辑顺序。
16.	Console在window最后一个，拖拽出来放到Tab（标签）里面可以调试script。![console][8] 
17.	Game Object有move to view和align with view可以从一个物体的视角去看。（官网有详细介绍）


  [1]: https://i.loli.net/2017/12/12/5a2f4af85ccf6.png
  [2]: https://i.loli.net/2017/12/12/5a2f5292a0c0c.png
  [3]: https://i.loli.net/2017/12/12/5a2f53366c64a.png
  [4]: https://i.loli.net/2017/12/12/5a2f549d54dc6.png
  [5]: https://i.loli.net/2017/12/12/5a2f54e922bb2.png
  [6]: https://i.loli.net/2017/12/12/5a2f675d0780b.png
  [7]: https://i.loli.net/2017/12/12/5a2f675f9148c.png
  [8]: https://i.loli.net/2017/12/12/5a2f675d61e0f.png
