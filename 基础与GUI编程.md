#Unity Basic&GUI Coding
----------
<style>
pre,img{
margin-left:30px;
}
</style>
> 此文档是对官网2D游戏基础，编程基础的知识总结，以及自己写推箱子和GUI代码的经验
###1.官网2D Game
- 2D游戏的层级排序，可以直接用layer，点击layer--add layer--sortinglayer,就可以用下面的layer覆盖上面的，然后在具体sprite上选择sorting layer（可以与他所属layer不同）。同一layer上order大的覆盖小的。</br>
<img src="https://i.loli.net/2017/12/20/5a3a1857870ad.png" alt="a.png" title="a.png"b height="150" />
- 精灵图导入后选择mutiple自动分割，可以在sprite editor里调节具体属性。</br>
<img src="https://i.loli.net/2017/12/20/5a3a1ad306dbe.png" alt="Z1F_BZ7`XFFM(~QJQ)NP)IS.png" title="Z1F_BZ7`XFFM(~QJQ)NP)IS.png" height="220"/>
- 碰撞边界：Box Collider 2D(长方形)/circle Coliider 2D(圆形)/polygon collider 2D(不规则)，可以点击edit collider手动调节。注意材质可以添加physics material来添加摩擦系数和弹性。
- 2D物理系统：Rigidbody2D,可以选择动态的，也可以选择Kinematic使物体不受重力系统影响单独使用脚本控制。减少重力影响调节Gravity Scale，减小质量调节mass,linear/angular drag是线性和角度阻力，使物理移动和旋转逐渐变慢。
<img src="https://i.loli.net/2017/12/20/5a3a2979c20ce.png" alt="`}}_BT4VY7%LTP9F~8]_)A0.png" title="`}}_BT4VY7%LTP9F~8]_)A0.png" height="220" />
###2.官网Scripting 初级
- 需要调节的值尽量使用public,运行脚本后仍旧可以调节，反复打开关闭脚本调节容易卡住。
- Awake()最先执行代码组件不是enable的时候也执行，Start()在第一帧update执行，只能在代码组件enable后执行。Update()每帧更新，执行完一帧执行下一帧，尽量不要在这里获取值以及持续执行一些耗性能的API。FixUpdate()同样每一帧都更新，但是每一帧间隔是固定的，适用于相对耗性能的对象，如有物理系统的。
- SetActive绑定在游戏对象上，该对象必须是勾选的状态，否则该脚本不会被执行。可以使用触发器解决这个问题，比如使用按钮的onclick绑定这个SetActive函数，点击触发激活。如果通过Find或类似方法寻找对象，对象也必须是被勾选的，因此通过其他非按钮对象把没勾选的对象激活不能用常规的方法，我在游戏里面是通过改变scale0到1实现的。enable方法只适用于灯光等组件，在每个组件的API里面会说明，大多数组件是不能使用enable开关的。需要注意的是，不勾选父元素,只有父元素的Actived是false，子元素的是true，父元素不显示是不能单独显示子元素的。
<pre><code>public void ToggleActive () {
gameObject.SetActive (!gameObject.activeSelf)
}</code></pre>
- 物体移动写法（只可以在没有绑定物理系统，或者物理系统是Kinematic）
<pre><code>public float moveSpeed=10f;
public float turnSpeed=50f;
void Update(){
//每一帧向Z轴走一步
transform.Translate(new Vector3(0,0,1));
//每一秒向Z轴走一步
transform.Translate(new Vector3.forward*Time.deltaTime);
//每一秒向Z轴按速度前进
transform.Translate(new Vector3.forward*moveSpeed*Time.deltaTime);
//按左键时每秒向左转
if(Input.GetKey(KetCode.LeftArrow))
transform.Rotate(Vector3.up,-turnSpeed*Time.deltaTime);
//按右键时每秒向右转
if(Input.GetKey(KetCode.RightArrow))
transform.Rotate(Vector3.up,turnSpeed*Time.deltaTime);
}</code></pre>
- 有缓冲的物体移动写法(GetAxis("Mouse X"),GetAxis("Mouse Y"),GetAxis("Mouse ScrollWheel"),GetAxis("Vertical "),GetAxis("Horizontal ")
更改缓冲重力选项在edit--project settings--input top menu
<pre><code>float h = Input.GetAxis("Horizontal");
float v = Input.GetAxis("Vertical");
float xPos = h * range;
float yPos = v * range;       
transform.position = new Vector3(xPos, yPos, 0);
</code></pre>
- Look At写法
<pre><code>public Transform target;
void Update(){
//这句话绑定到摄像机后，摄像机可以一直跟踪target,注意有local和global两种模式可以切换
transform.LookAt(target);
}</code></pre>
- Lerp的用法(返回前两个值之前的某个值)
<pre><code>// In this case, result = 4
float result = Mathf.Lerp (3f, 5f, 0.5f);

Vector3 from = new Vector3 (1f, 2f, 3f);
Vector3 to = new Vector3 (5f, 6f, 7f);
// Here result = (4, 5, 6)
Vector3 result = Vector3.Lerp (from, to, 0.75f);

//光线逐渐（每秒）强度趋近8f
light.intensity = Mathf.Lerp(light.intensity, 8f, 0.5f * Time.deltaTime);
</code></pre>
- Destory可以摧毁一个物体，之后这个脚本也会不可用，可以使用 Destroy(GetComponent&lt;MeshRenderer&gt;())来停止渲染或者把destory脚本放到其他的对象上。
<pre><code> //在1.5秒后摧毁物体
Destroy (gameObject, 1.5f);
</code></pre>
- 引用其他脚本（本质上一个脚本是一个class,就是在调用其他class），每个脚本应该只做一件事情，一个物体需要多个事件就绑定多个脚本；脚本尽量写的通用，在更换对象的时候可以复用，比如使用struct和泛型。
<pre><code>public GameObject otherGameObject;  
//脚本类型就是自己的名字 
private AnotherScript anotherScript;
private YetAnotherScript yetAnotherScript;
private BoxCollider boxCol;   
void Awake ()
 {
   anotherScript = GetComponent<AnotherScript>();
   yetAnotherScript = otherGameObject.GetComponent<YetAnotherScript>();
   boxCol = otherGameObject.GetComponent<BoxCollider>();
 }   
void Start ()
{
   boxCol.size = new Vector3(3,3,3);
   Debug.Log("The player's score is " + anotherScript.playerScore);
   Debug.Log("The player has died " + yetAnotherScript.numberOfPlayerDeaths + " times");
}
//stuct案例(stuct 是值类型，在一个脚本里面可以使用多个stuct,让你的数值逻辑清晰)
 public Stuff(int bul, int gre, int roc)
{
   bullets = bul;
   grenades = gre;
   rockets = roc;
}
 public Stuff myStuff = new Stuff(10, 7, 25);
</code></pre>
- 生成实例,比如子弹和小怪 Instantiate
<pre><code>public Rigidbody rocketPrefab;
    public Transform barrelEnd;
    void Update ()
    {
        if(Input.GetButtonDown("Fire1"))
        {
            //使用2D物理系统
            Rigidbody rocketInstance;
            //生成火箭，在火箭筒尾部，以火箭筒尾部的角度，使用物理系统
            rocketInstance = Instantiate(rocketPrefab, barrelEnd.position, barrelEnd.rotation) as Rigidbody;
            //加入发射的力（向量），火箭筒尾部前方角度的向量*5000长度
            rocketInstance.AddForce(barrelEnd.forward * 5000);
        }
    }
</code></pre>
- Invoke(延时方法，和settimeout setintervial一样，只能调用没有返回值的方法）
<pre><code>//3秒后执行settimeout
this.Invoke("setTimeOut", 3.0f);
//两秒后执行setintervial,之后每秒重复执行
this.InvokeRepeating("setInterval", 2.0f, 1.0f);
</code></pre>
- enum(枚举数据类型的应用)
<pre><code>enum Direction {North, East, South, West};
    void Start () 
    {
        Direction myDirection;        
        myDirection = Direction.North;
    }    
    Direction ReverseDirection (Direction dir)
    {
        if(dir == Direction.North)
            dir = Direction.South;
        else if(dir == Direction.South)
            dir = Direction.North;
        else if(dir == Direction.East)
            dir = Direction.West;
        else if(dir == Direction.West)
            dir = Direction.East;     
        return dir;     
    }
</code></pre>
###3.官网Scripting 中级
- 属性get/set方法同C#，如public int Health{ get; set;}，myplayer.Health=100;
- 三元操作符，如message = health > 0 ? "Player is Alive" : "Player is Dead";
- 静态值，静态方法和静态类都是非常实用的，比如使用静态值计数敌人，使用所属class.静态值名字访问这个值，使用所属class.静态方法名，直接调用这个方法。需要注意的是静态类的本质，是一个抽象的密封类，所以不能被继承，也不能被实例化。 使用静态类的优点在于，编译器能够执行检查以确保不致偶然地添加实例成员，编译器将保证不会创建此类的实例。静态成员只被创建一次，所以静态成员只有一份，既静态变量的值是唯一的。成员需要被共享的时候，方法需要被反复调用的时候，就可以把这些成员定义为静态成员。在静态方法中，不能直接调用实例成员，因为静态方法被调用的时候，对象还有可能不存在。
- 参数类型或者数量不同的的方法可以使用同一个名字，比如ADD数字和ADD字符串和ADD多个数字或字符串。这样调用这个类实例的ADD方法时，只要输入不同类型的参数就可以调用对应的方法。
- Unity可以使用泛型类class<T>,在实例化的时候，给这个class一个类型，就可以调用里面的所有泛型方法。当然，单独使用泛型方法也没有问题。

###4.个人经验
- 首先，导入全部素材后，做自己项目的时候，一定要在Project新建一个文件夹，把用到的素材，自己的代码和场景放在一起，做完之后就可以把不用的素材删除了。要不然你就要忍受巨大的游戏包，或者冒着删掉自己内容的风险。
- for不要超过两次，有递归的话，两层for外面递归。把每个功能尽量独立开来写，在update里面调用。
- 要注意尽量用local scale和local pozision，只在合适的时候使用协程和静态成员。Mono在Unity5.5+已结升级到mono4.6+，因此之前的foreach等问题都解决了，在查看网上资料Unity坑和内存的时候，很多内容都已经不适用了，尽可能看官网或者论坛吧。
