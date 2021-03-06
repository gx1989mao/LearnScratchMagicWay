由于Scratch是一款面向少儿的编辑器，为了避免儿童的随意使用造成软件崩溃，引入了许多不够完善的特性。刚接触时往往会陷入一些奇怪的疑难问题之中。本章将会从以下几个方面介绍日常使用Scratch会碰到的边角问题：

1. 绘图和外观的坑
2. 看不见摸不着的事件控制
3. 不知道该用什么变量的克隆体
4. Scratch的谜之容错
5. 拖慢脚本的罪魁祸首

# 1. 绘图和外观的坑

1. 任何图片都不能离开舞台
2. 矢量图和位图切边
3. 角色和克隆体的复杂层级关系
4. 默认角度90°
5. 消失的几种姿势

#### 任何角色都不能离开舞台

为了防止不小心把角色移动出舞台找不到，scratch做了几个限制。首先任何角色在移动到舞台边缘的时候会强制留下一个边缘，刚好可以用鼠标拖拽它回到舞台之中。例如我们使用坐标移动积木让某角色移动到x1000,y0。这样角色实际上的x坐标在250多就已经卡住了，刚好在舞台上漏了一个角。

对于角色的限制不光在于位置，还包括了角色的外观大小。我们让角色大小设定为极大例如1000，在外观积木盒子最下方的大小变量之上用鼠标勾选之后，可以观察到，角色根本没有变成1000，而是卡在了几百就不在增加。

相应的，如果我们给角色设定一个1的大小甚至-1000的大小，角色大小就被锁定成了5。这是为了防止角色太小无法画在舞台上。

理解了scratch的这个角色不能离开舞台的机制之后，我们就可以考虑一些问题。如果想让一个克隆体从画面外移动进画面，这个动作可以做到么？答案是不能。因为角色一旦设置一个画面外的位置，scratch就会强制把角色的一个小角拉到舞台上，这样画面边界一定会留一点角色。根本无法让角色完全从舞台外移入舞台。

![](/assets/P1c/P1_challange/P1c 强制拖入舞台.png)
###### 图 不能完全从舞台外移动进舞台

如果我们一定要让角色从舞台外移动进舞台，就要把入口这个边界用一个不透明的角色遮盖住。这样在视觉效果中，就看不到这里的异常了。例如我们想用scratch做一个横版卷轴游戏，类似超级马里奥。地图的卷动就需要从舞台外移动进来，这时候就需要把边界遮住防止玩家看到这里堆了奇怪的东西。

#### 矢量图和位图切边

首先要界定什么是矢量图什么是标量图（位图）。我们做如下界定：

1. 矢量图是指软件自己知道如何可以画出来的图，可以一条线一条线的画出来
2. 标量图是软件只拿到了一副画，这幅画被毁掉了软件也不会补充回来了

关于矢量和标量的定义和意义大家如果有兴趣请参看part3的数学概念

scratch对于外形自带了两种编辑模式可以互相切换，不过非常不建议各位读者使用这些编辑功能，因为十分不完善的编辑功能影响体验。这里要强调的是几个原则：

1. 从本地导入的图片，即时原来是所谓的“矢量图”格式，例如png等。导入scratch之后也默认变成了一个位图，因为它并不是scratch自己画的
2. scratch承认的矢量图，只有两类。一类是造型之中切换矢量图编辑模式自己绘制的图片，另一类是scratch角色库之中自带的角色
3. scratch承认的矢量图在造型之中编辑。如果不小心被我们在编辑中移动出画布，还可以移动回来。但是本地导入的图片如果不小心移动出编辑画面就会发生切割丢失现象。

为什么会发生切割丢失现象呢？原因是图片移出了编辑区之后都会发生切割，但是矢量图scratch知道怎么画回来，而位图scratch不知道怎么画于是就丢失了。如何避免这个情况发生呢？这里我们要首先使用位图编辑的选择工具选中图片的主要内容，通过缩放和拖拽来设置图片中心。防止在操作过程中让图片移出编辑区，这样就不会发生切割丢失现象了。

![](/assets/P1c/P1_challange/P1c 防止切割丢失的方法.png)
###### 图 如何避免设置中心时候发生切割丢失现象

#### 角色和克隆体的复杂层级关系

在scratch之中有一种复杂的层级关系和管理原则

1. 角色在舞台上被拖拽就会默认移到顶层。
2. 舞台无论如何都在最下层，画笔和图章都会绘制在舞台上。换句话说任何不透明角色都可以挡住画笔和图章。
3. 任何新创建的角色都会默认移到顶层。
4. 角色的克隆体默认在角色下一层。
5. 角色再次克隆的克隆体再角色和前一个克隆体之间。因为首先它是新生成的物体放在最顶层，其次执行角色本体在克隆体之上的操作。

![](/assets/P1c/P1_challange/P1c 层级关系.png)
###### 图 角色和克隆体层级关系

#### 默认角度90°

scratch之中角色的默认角度是90°，我们在导入图片制作角色时一定要注意尽量让俯视图片的头部方向朝右。例如我们要做一个火箭，火箭的头部如果朝上，后果就是我们用移动积木和方向积木来控制火箭时，火箭永远都是横着飞，这是我们尽量要避免的。
但是如果是横版角色，例如scratch初始化的那只猫，就不存在这个问题了，因为它只有左右两个方向。

#### 消失的几种姿势

scratch之中可以通过三种方式让一个角色从我们眼前消失：

1. 隐藏，最常用的方法，但是要注意隐藏是一个下次运行仍旧会保存的状态，要和显示积木配合使用。隐藏了之后无法判断碰撞其他角色，但是请注意，这时候仍旧可以在隐藏角色身上判断碰撞边缘和鼠标指针。这是scratch的特殊细节。
2. 透明度特效，在外观特效里面可以用透明度特效让角色消失，这时候我们仍旧可以让角色判断和别的角色的碰撞，因为他还在舞台上。这样做的副作用是带来运行速度的减慢，如果有大规模的克隆体请尽量避免使用这种方法防止运行卡顿。
3. 选取角色图片和背景颜色相同，角色自然就隐藏起来了。有些区域我们想看到角色就把该区域的背景换成不同颜色即可。这种方法十分巧妙，并不增加scratch的运行负担而且仍旧可以正常判断碰撞。

# 2. 看不见摸不着的事件控制

1. 鼠标点绿旗的迷惑
2. 消息等待
3. 等待0秒？等待-1秒？最小等待时间是多少0.033平均
4. 藏起来就不刷新屏幕的列表和变量 没有anyone刷新屏幕就飞快
5. 死循环嵌套的后果
6. 计数循环变计数能否进行
7. 拖拽优先级最高
8. 并行的条件竞争 碰到边缘反弹和等待碰到边缘
9. 真实时间的获取

#### 鼠标点绿旗的迷惑

在scratch之中，绿旗是我们最常使用的入口。但是如果我们让角色在鼠标按下的时候做出一些动作（例如发射子弹），这时候问题就来了。一旦我们点击绿旗开始还没准备好真正开枪，scratch会马上发射一个子弹。这是因为scratch在绿旗入口的瞬间抓到了鼠标按下的事件。为了解决这个怪异的行为，我们必须使用一个称之为锁的东西。
scratch给我们提供了很方便的实现锁的工具，那就是“等到直到”积木。我们只需要在绿旗入口下跟一句：等到直到鼠标键按下不成立。这样在我们点击绿旗之后，程序会等待直到我们把鼠标键松开，才继续向下运行。就好像一个锁把程序运行的脚步锁住。这个概念我们将会在后文经常用到。
在真正的文本语言之中，锁的实现是用变量记录状态做的。这种方法略显复杂。在scratch之中经常使用的就是等待直到积木。

###### 绿旗点击锁

#### 消息等待

在scratch之中，我们经常会接触到消息发送接受。最常见的错误就是在消息等待过程之中使用死循环。“广播消息并等待”积木将会等待所有的消息接收者执行完毕，如果有一个接受者身上带有一个死循环重复执行。那么它将永远无法执行完毕，广播消息并等待的积木也将天长地久的等下去无法继续运行后面的内容。

#### 等待0秒？等待-1秒？最小等待时间是多少0.033平均

在scratch之中我们最容易搞不清楚的就是等待1秒积木。其实等待积木和任何重复执行积木都在一次运行或者一次循环之后会将运行时间补充到0.33秒。如果我们将等待积木写等待0秒或者等待-1秒，再测量其运行时间，结果都将是0.33秒左右。

![](/assets/P1c/P1_challange/P1c 等待0秒.png)
###### 等待0秒

#### 藏起来就不刷新屏幕的列表和变量 没有anyone刷新屏幕就飞快

是不是scratch的所有重复执行都会补充时间到0.33秒呢？这里是有例外的。
如果我们把重复执行之中只放一个操作列表的语句，例如加入一个元素。然后我们先将列表隐藏，点击绿旗运行大约1秒时我们打开列表显示观察会发现，列表的元素有好几十万。这是因为重复执行积木中没有任何要刷新屏幕的积木，所以默认全速运行，也不会补充时间。
如果我们把列表显示出来，这时候再运行，发现列表元素增加的过程慢了许多。这是因为列表的显示和更新就是刷新屏幕的动作，重复执行积木会补充时间。

![](/assets/P1c/P1_challange/P1c 列表刷新屏幕.png)
###### 列表和重复执行刷新屏幕

#### 死循环嵌套的后果

新手经常犯的错误还包括死循环嵌套死循环。外层的死循环运行第一次的时候就进入了内层的死循环，从此陷入了内层的死循环再也无法出来。也就是说，外层循环变成了一个摆设，因为就运行了一次根本不会循环。请大家一定要注意。

![](/assets/P1c/P1_challange/P1c 死循环嵌套.png)
###### 死循环嵌套

#### 计数循环变计数能否进行

假如我们用一个变量来控制计数循环，如果这个变量在循环之中被改变了，那么循环次数会动态的改变么？答案是，不会。计数循环在第一次运行时就已经被锁定了运行次数，无论我们怎么改这个变量，循环次数是不会发生改变的。这是为了防止出现一些运行错误。

![](/assets/P1c/P1_challange/P1c 计数循环.png)
###### 变量控制计数循环

#### 拖拽优先级最高

在scratch的编辑模式（就是平常的模式，相对于全屏播放模式来说的），运行程序时，拖拽的优先级是高于点击判断的。如果我们程序中设定了点击角色之后有一些操作，那么点击角色之后会首先把角色拖拽起来，而不是执行这些操作。
假如我们要做一个画笔，点击鼠标之后落笔开始画。点击画笔之后首先会拖拽起来画笔图案，而不是首先落笔。
解决这样的问题有两个办法，第一个是永远在播放模式下运行，且不去勾选播放模式可以拖动的选项，这样就不会出现拖动的情况。第二种是将画笔图案的中心设置在图案外，这样在点击和移动到鼠标时，鼠标不会碰到图案，从而不会产生拖拽。巧妙地解决问题。

![](/assets/P1c/P1_challange/P1c 拖拽画笔.png)
###### 图案中心设置解决拖拽问题

#### 并行的条件竞争 碰到边缘反弹和等待碰到边缘

在scratch之中，每一个程序入口下的脚本都是并行的。这样就会产生竞争，如果两个并行的脚本同时操纵一个角色，就会改来改去发生错误。例如我们在角色身上写两个脚本，一个让角色重复执行移动，碰到边缘就反弹。另一个脚本重复执行判断是否碰到边缘，碰到边缘就让角色说：“碰到边缘”。
这样的运行，偶尔角色会说碰到边缘，偶尔不会说。就是因为发生了竞争，碰到边缘的时候：A）如果碰到边缘反弹的积木先执行，角色脱离了边缘才进行碰到边缘检测说话，这时候不会说话。B)如果碰到边缘检测说话先执行，那么角色就会说话。
A和B两种情况我们并不知道哪个会先发生，这就是并发竞争。
需要注意的是，这种竞争产生的bug偶尔会出现偶尔不会出现，十分难以察觉，属于最难搞定的bug之一。

![](/assets/P1c/P1_challange/P1c 碰到边缘反弹竞争.png)
###### 并发竞争脚本

#### 真实时间的获取

在scratch之中，有许多和时间有关的积木，我们来看一下他们之间的区别。

1. 等待1秒积木
2. 计时器
3. 自2000年至今的天数

最不准确的积木就是等待1秒，前面我们已经说过，该积木会带来三十分之一秒的刷新延迟。本身也不适合记录精确时间。
计时器看上去比等待1秒积木靠谱的多，但是依旧有一个问题，如果我们的程序有些庞大，运行速度整体变慢，计时器也会变得很卡，这时候时间也不做准。
最准确的时间是自2000年至今的天数，这个积木块用的是计算机时间，我们把这个积木乘以100000，就会得到秒级别的真实时间。当然我们可以获得更小的时间分辨率，但是意义不大。请大家自己探索。


# 3. 不知道该用什么变量的克隆体

1. 私有变量和私有改动
2. 消息入口的继承
3. 使用消息指数克隆
4. 网格锁定
5. 克隆体整理
6. 克隆体克隆别的克隆体的队列设置 

scratch的克隆体是不大容易理解的概念，其中经常使用的一些私有和公有变量的概更是抽象。下面我们来例举一些常见的使用方法。

#### 私有变量和私有改动

在克隆体之中，我们为了独立控制每个克隆体，经常会给克隆体设置私有变量。实际上我们是把私有变量设置在母体身上，当克隆动作执行时，克隆体身上也复制了这个私有变量，但是这个私有变量就是属于克隆体的，和母体再无关系。

实际上克隆会复制以下事情：

1.母体角色当前的外观造型
2.母体角色当前的位置坐标
3.母体身上全部的脚本
4.母体身上全部的私有变量和私有列表

前两条容易理解，第三条说克隆体复制了母体的全部脚本，那么当作为克隆体启动这个入口的意义在哪里？
其实克隆体也复制了母体身上当绿旗被点击入口的脚本，只是绿旗这时候没有被点击所以不满足这个入口条件不会运行，而作为克隆体启动入口这时候刚好满足条件可以运行。
如果母体身上带有一些消息入口，克隆体同样会复制下来，当消息广播时同样可以运行。我们可以通过消息来控制克隆体。
第四条说母体身上全部的私有变量和私有列表。那么公有的呢？答案是公有的不需要刻意复制，谁都可以自由使用。而私有变量如果不复制，在别的角色身上是看不到没法用的。

另一个关键的事情是：积木盒子之中角色的xy坐标、方向、造型编号等可以打钩显示的变量，全都默认是私有变量。请大家注意区分，私有变量显示时有“XXX：变量名”，而公有变量只有变量名。

#### 消息入口的继承

刚才提到了克隆体可以复制母体的消息入口，这个机制可以很方便的实现克隆体的控制。例如我们要某个消息可以把所有的克隆体清除掉，那么简单的在母体身上写下脚本：接收到消息删除本克隆体，这样就可以做到。
这里的消息可以帮我们实现许多集体效果。例如我们想通过一个消息让所有的克隆体震动一下，做出一个炫酷的效果。我们可以让消息入口下跟一个x坐标增加-20，等待0.1秒后x坐标增加20。其他用法请大家自己探索。

#### 使用消息指数克隆

如果我们想通过很少的代码大量的克隆角色，常用的方法有：

1.重复执行+克隆某角色（运行速度慢有卡顿感）
2.把重复执行+克隆做成一个运行时不刷新屏幕的自定义积木
3.让克隆体克隆自己

这里主要说一下第三个方法。学过幂指数就应该知道。如果我们让克隆体作为克隆体产生时克隆自己，克隆体将会以2的指数来增长。scratch为了防止克隆体太多拖慢速度，会限制同时存在的角色总数目为300个。

我们还可以用消息来控制角色的克隆过程，为了区分克隆出来的人，我们让本体接收到消息时左转移动并克隆自己，让克隆体启动时向右移动。这样克隆体也继承了这个消息入口，再次发送这个消息时，刚才的克隆体也会分裂出下一代的克隆体。

![](/assets/P1c/P1_challange/P1c 二叉树.png)
###### 二叉树克隆

#### 网格锁定

如果我们想让克隆体在播放时可以被拖动，但是放开鼠标按键的时候我们想让角色固定在我们约定的网格上。例如我们想做一个拼图，希望拖拽放手时拼图块可以自动对齐。
这时候我们就要用坐标取整的方法了。首先打开角色的播放时可以被拖动开关，然后在克隆体身上加上取整移动。

![](/assets/P1c/P1_challange/P1c 网格锁定.png)
###### 坐标取整

#### 克隆体整理

有时候我们想用克隆体做一个整齐的砖墙，这种位置有规律的克隆体应该怎么做呢？

基本的思路是：

1. 生成位置变量
2. 重复执行克隆角色
3. 更新生成位置变量

这里面有许多讲究和细微差别，首先如果我们用重复执行来克隆多个角色，重复执行本身是有0.33秒的固定刷新时间的，如果我们要克隆一百块的砖墙，就会耗费三秒钟。如果我们想快速的刷新就一定要使用自定义积木把重复执行克隆角色的动作包进去，并且运行时不刷新屏幕。
这时候问题就来了，例如我们要克隆一排角色，每隔10步摆放一个。如下图所示的积木

![](/assets/P1c/P1_challange/P1c 克隆体外更新.png)
###### 自定义积木重复执行 克隆体外更新位置变量

这样运行的结果是，克隆出来的十个角色都叠在了一起，并没有按我们预想的排成10步为间隔的一排。这究竟是什么原因呢？
其实scratch执行一次克隆角色到作为克隆体积木启动之间是有一个比较大的时间差的，我们可以用如下的积木去测量这个时间差。

![](/assets/P1c/P1_challange/P1c 克隆时间.png)
###### 作为克隆体启动入口执行时间

可以观察到克隆动作到作为克隆体启动之间大概平均会耗费0.003秒左右，大概是重复执行刷新屏幕时间的十分之一左右，而且这个时间差并不准确，每次执行会变动。但是无论如何，这个时间和计算机全速运行的时间相比是很长的。
换句话说我们运行时不刷新屏幕的全速运行重复执行克隆动作，每一个克隆动作开始时刻的差别很小，几乎可以认为是同时开始，更新变量posi的值瞬间已经被算好了结果是100。耗费0.003秒左右所有克隆体的作为克隆体被启动的入口才慢吞吞的开始执行，他们能读取到的变量posi值已经等于100了。所以所有克隆体都把自己移动到了100的位置。
我们该如何解决这个问题呢？很简单，更新变量放在作为克隆体启动入口下。因为所有的克隆体本身就有一个顺序在，虽然每次的克隆动作启动几乎是同时的，但是还是有顺序差别。让他们每次作为克隆体启动时更新位置变量，就可以让角色按我们预想的顺序间隔排列了。

![](/assets/P1c/P1_challange/P1c 克隆体内更新.png)
###### 自定义积木重复执行 克隆体内更新位置变量

上面讨论的是克隆一行角色，是一维情况。如果我们想克隆一个二维的砖墙，应该怎么做呢？
如果不考虑运行速度，为了代码的简约，我们会采用如下方法来实现：

![](/assets/P1c/P1_challange/P1c 二维克隆.png)
###### 双变量 克隆体外更新

如果我们想追求运行速度，用自定义积木不刷新的方法来实现，就一定要用克隆体内更新变量的方法来实现。又因为我们有两个变量要更新，这里我们就要加入一个“算式”来计算。

![](/assets/P1c/P1_challange/P1c 二维克隆 克隆体内更新.png)
###### 双变量 克隆体内更新 算式

可以看到算式十分的麻烦而且每次更改十分不便。这时候我们就需要进一步抽象，使用列表先生成位置，所有的角色更新变量只对应了列表的编号。这样只需要用一个变量就解决了所有的问题，而且在我们改动了生成的规则时，不需要再更改每一个角色身上的“算式”。这样比较容易修改。

![](/assets/P1c/P1_challange/P1c 列表克隆.png)
###### 单变量 克隆体内更新 列表编号更新

#### 克隆体克隆别的克隆体的队列设置

如果我们要让一个克隆体克隆另一个克隆体，例如被克隆出来的敌机，要克隆发射子弹。最直接的思路是敌机更新一个xy的位置变量，接着马上克隆子弹。子弹作为克隆体启动时就移动到xy这个位置。但是这里面存在着并行的竞争问题，如果两个克隆敌机在很短时间内几乎同时发射了子弹。A敌机先更新xy变量，并克隆子弹，子弹作为克隆体启动的时间内，xy被B敌机更新。本该从A射出的子弹结果却从B射出。
解决这样的竞争我们需要用到一个排队的思想。敌机每次发射时先把xy这个发射位置写入一个数组，舞台上的脚本一旦发现数组之中有元素就按顺序克隆一个子弹，之后把这一条发射位置删掉。这样提出发射要求和处理发射要求就被分开了。就好像我们去买东西，要买东西的人（要发射子弹的敌机）都要排队，而售货员（舞台）负责卖给你东西（克隆子弹）。这样就不会出现竞争的问题。

![](/assets/P1c/P1_challange/P1c 敌机逻辑.png)
![](/assets/P1c/P1_challange/P1c 敌机子弹逻辑.png)
###### 队列克隆子弹发射

需要注意的是，克隆体克隆别的克隆体时，要尽量谨慎的使用运行不刷新的自定义积木。否则会导致scratch运行卡顿。


# 4. Scratch的谜之容错

1. 不写入数字会如何
2. 写入小数的列表提取
3. 造型编号0的问题 颜色和角度的小于零
4. 积木空缺填补参数 面向角色的积木块可以加入变量
5. 利用容错能做什么 

#### 不写入数字会如何

在许多计算机语言中，都存在默认值问题。也就是当我们定义了一个变量，又没有给变量赋值。这时候变量值是多少呢？为了防止发生错误，我们就要给一个默认的数值。scratch之中，数值变量的默认值是0，布尔变量的默认值是false。
在运算积木盒子里面，所有的空格都是默认值。例如加法积木，我们如果不填入内容就默认是0，与或非积木不填入的空格也默认是填入了false。也就是说什么都不填的与或双击运行结果是false，不成立积木双击运行结果是true（因为默认填入的是false）。

![](/assets/P1c/P1_challange/P1c 默认运行.png)
###### 直接运行积木结果

除法积木和余数积木因为分母不能为零，所以什么都不写入直接双击运行结果是NaN代表了没有结果返回。
比较复杂的是大小判断积木。大于小于积木在什么都不填入的情况下双击运行结果是false，而等于判断积木在什么都不填入时双击结果是true。

#### 写入小数的列表提取

我们可以在取出列表某一项元素时使用小数，例如取得列表的1.345项，这时候scratch会把1.345向下取整为1，然后取出第一项。如果我们试着访问列表的第0项或者负数项，又或者是访问超过列表项目数的项，返回的结果都是空白。

#### 造型编号0的问题 颜色和角度的小于零

在scratch之中，有一些可以循环访问的东西。包括：

1. 角色的造型编号
2. HSV的颜色数值
3. 角色方向

例如角色造型编号如果写入0，则是切换到角色最后一个造型，以此类推的-1是倒数第二个造型。而颜色超过了200则相当于又从零开始，小于零相当于从200倒数。方向也是同样。这些可以避免许多使用时的错误。

#### 积木空缺填补参数 面向角色的积木块可以加入变量

在将造型切换为积木中，造型不止可以通过下来菜单来选择，还可以放一个变量来控制。这个变量可以是数字也可以是字符串。如果是数字的话，对应的就是造型的编号，如果是字符串对应的就是造型名称。同时有数字和字符串时，造型编号会优先于造型名称。

#### 利用容错能做什么

如果我们要分辨一个变量是数字还是字符串，我们就可以用变量乘以1，观察变量是否等于变量本身。scratch之中字符串乘以1的容错结果是0，而数字乘以1会得到数字本身。这样我们就可以简单的判断变量是否是数字了。

# 5. 拖慢脚本的罪魁祸首

在scratch之中，我们要尽量避免三种用法：

1. 特效加循环
2. 外形复杂多角色移动
3. 克隆大量角色

原因是这些动作会耗费大量的计算机资源，如果我们还同时使用自定义积木的运行时不刷新屏幕选项，scratch会直接卡死崩溃，请尽量避免。


