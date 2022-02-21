## 11.24
### webpack配置路径别名
webpack.base.conf.js
resolve -> alias -> 配置项
例子：
```
alias: {
    'vue$': 'vue/dist/vue.esm.js',
    '@': path.resovle(__dirname, 'src'),
}
```
### transition(过渡)

transition-property
transition-duration
transition-timing-function
transition-delay

### animation(动画) + @keyframes

animation-name
animation-duration
animation-timing-function
animation-delay
animation-iteration-count
animation-direction

@keyframes ani {
    20% { opacity: 0.3; }
    40% { border-readius: 100px; }
}

### 3D地图
创建地图
```
const map = new FMap3D.Map(el, {
    zoom: 16, 
    center: [116.461753, 39.908391], 
    pitch: Math.PI / 3 //地图倾斜角度
})
```
鼠标事件:
地图容器接受到鼠标事件之后，会被分发给不同的layer, layer根据事件做出响应。


```
layer.on('click', (feature, event, lnglat) => {
    if(feature) {
        feature.setStyle({
            fill: {
                color: 'red'
            }
        })
    }
})
```

### onclick与addEventListener区别

onclick事件在同一时间只能指向唯一对象
addEventListener可以给一个事件注册多个listener
addEventListener对任何DOM都是有效的，而onclick仅限于HTML
addEventListener可以控制listener的触发阶段，(捕获/冒泡)。对于多个相同的事件处理器，不会重复触发, 不需要手动手机用removeEventListener清楚
IE9使用attachEvent和detachEvent

addEventListener(event, function, useCapture)
useCapture: true(捕获) false(冒泡)

### js原生属性方法

getComputedStyle(element, pseudo-element) 返回当前元素所有最终使用的css属性值
getPropertyValue("color") 取出元素最终计算出的css样式
window.getComputedStyle(a).getPropertyValue("color");

### foreach和map的区别

foreach不会返回执行结果，而是undefined,foreach会修改原来的数组。而map()方法会得到一个新的数组并返回

## 11.25
### FGL
FGL提供两种渲染器:

WebGL渲染器: 将一般物体用WebGL方式渲染到画布上
DOM渲染器: 将普通DOM元素置于正确的世界坐标系下

光源种类:
环境光: 不产生阴影，改变物体的颜色
平行光: 太阳光
聚光灯: 圆锥形的光线
点光源: 球状光线

物体:
由Geometry,Material组成

## 11.26
### WebGL绘制流程
1. 准备数据阶段
2. 生成顶点着色器
3. 图元装配(画出一个个三角形) 顶点坐标 -> 图元
4. 生成片元着色器
5. 光栅化(生成像素点，生成片元) 图元 -> 片元

#### 获取顶点坐标
三维软件导出顶点坐标 -> 将顶点坐标写入缓存区

#### 图元装配
顶点坐标 -> 传入顶点着色器 -> 图元装配

顶点着色器会先将坐标转换完毕，然后由GPU进行图元装配，有多少顶点，这段顶点着色器程序就运行了多少次。

#### 光栅化

图元 -> 生成片元 -> 片元 -> 光栅化 -> 片元

#### 片元着色器处理流程

图元 -> 传入顶点 -> 片元着色器 -> 光栅化
片元着色器是生成多少像素，运行多少次

### Three.js
#### Three.js具体流程
创建场景 -> 配置场景 -> 创建模型 -> 创建材质 -> 生成着色器 -> 渲染

#### Three.js目录结构
```
/build three源码，一份压缩，一份未压缩
/docs 帮助文档, 可以查看构造函数，方法，属性的属性说明
/editor 可视化编辑器，创建在线预览的三维场景
/examples 各种三维的demo
/src three.js引擎每一个主要构造函数对应的源代码
```

### CDN
CDN是建立并覆盖在承载网之上，由分布在不同区域的边缘节点服务群组成的分布式网络
CDN支持多种场景加速: 图片小文件，大文件下载，音视频点播，直播流媒体，全站加速，安全加速

CDN全过程:
1.当终端用户向www.a.com下的指定资源发起请求时，首先向本地DNS发起域名解析请求
2.本地DNS检查缓存中是否有www.a.com的IP地址记录。如果有，则直接返回给终端用户，如果没有，则向授权DNS查询。
3.当授权DNS解析www.a.com时，返回域名CNAME www.a.tbcdn.com对应的IP地址。
4.域名解析请求发送至阿里云DNS调度系统，并为请求分配最佳节点IP地址
5.LDNS获取DNS返回的解析IP地址。
6.用户获取解析IP地址
7.用户向获取的IP地址发起对该资源的访问请求

## 11.29
### three.js
#### 渲染要素
几何体 Geometry
场景对象 Scene
网络模型 Mesh
光源 点光源 环境光
相机设置 
创建渲染器对象

#### 动画
requestAnimationFrame
专门为动画设计的API，不展示的时候不会渲染，与浏览器重绘的频率一致

#### 鼠标操作三维场景旋转缩放
three.js控件OrbitControls.js
可以控制视角，进行旋转缩放平移

#### 光照设置
AmbientLight 环境光
PointLight 点光源
DirectionalLight 平行光
SpotLight 聚光源

#### 顶点
BufferGeometry
1. geometry.attributes 
attributes.postion
attributes.color
attributes.normal
attributes.uv
attributes.uv2
2. geometry.index 顶点索引属性
3. BufferAttribute 对象

## 11.30
#### 照相机
透视投影相机 (远大近小)
THREE.PerspectiveCamera(fov, aspect, near, far)
fov: 张角, aspect: canvas的宽高比 
正交投影  (远近都一样大)

Geometry本质都是一个一个三角形拼接而成的,所以可以通过设置三角形的法线方向向量来表示集合提表面各个位置的法线方向向量

#### 常用材质介绍
点材质: PointsMaterial
线材质: LineBasicMaterial LineDashedMaterial 
网格材质: MeshBasicMaterial MeshLambertMaterial MeshPhongMaterial MeshStandardMaterial MeshPhysicalMaterial MeshDepthMaterial MeshNormalMaterial
精灵Sprite材质: SpriteMaterial
自定义着色器材质: RawShaderMaterial ShaderMaterial

#### 层级模型和组对象
对于每个几何物体，我们可以将其组合成一个Group,Group又可以组成新的Group从而形成树状结构，对多个几何物体进行相同的操作，并可以进行一系列遍历。

## 12.1
### Geometry渲染参数
#### CubeGeometry
width, height, depth, widthSegments, heightSegments, depthSegments
#### PlaneGeomtry
width, height, widthSegments, heightSegments
#### SphereGeometry
radius, segmentsWidth, segmentsHeight, phiStart, phiLength, thetaStart, thetaLength
#### CircleGeometry
参数类似球体
#### Cylinder
radiusTop radiusBottom height radiusSegments heightSegments openEnded
#### 正多面体
指定半径就行
#### ToursGeometry
圆环面 
radius 圆环半径 tube 管道半径 

### 曲线
#### ArcCurve(圆弧线)
ArcCurve(aX, aY, aRadius, aStartAngle, aEndAngle, aClockwise)
getyPoints() 按照一定的细分精度返回沿着圆弧线分布的顶点坐标,返回一组由二维向量或者是三维向量的数组

#### 样条曲线和贝塞尔曲线
样条曲线设置多个顶点，形成一条光滑的曲线
贝塞尔曲线有两个起始点和一个控制点,二次贝塞尔曲线有两个起始点和两个控制点

CurvePath可以把多个圆弧线,样条曲线，直线等组合成一个曲线

TubeGeometry 曲线路径管道成型

LatheGeometry 旋转成型
LatheGeometry(points, segments, phiStart, phiLength)
points  坐标数据组成的数组
segments  圆周方向细分数,默认12
phiStart 开始角度,默认0
phiLength 旋转角度,默认2pai

ShapeGeometry 轮廓填充
根据轮廓的顶点使用三角面Face3自动填充中间区域

```
var shape = new THREE.Shape(points);
var geometry = new THREE.ShapeGeometry(shape, 25);
```

ExtrudeGeometry 拉伸成型
五点可以描述一条曲线,再进行拉伸
```
var geometry = new THREE.ExtrudeGeometry(
    shape, //二维轮廓 
    //拉伸参数
    {
        amount: 123, //拉伸长度
        bevelEnabled: false 
        
    }
)
```

## 12.3
### 纹理贴图
纹理贴图加载器TextureLoader的load方法加载一张图片可以返回一个纹理对象Texture,纹理对象Texture可以作为模型材质颜色贴图.map属性的值
```
var textureLoader = new THREE.TextureLoader();
textureLoader.load(url, function(texture) {
    let material = new THREE.MeshLambertMaterial({
        map: texture,
    })
});
let mesh = new THREE.Mesh(geometry, material);
scene.add(mesh);
```

图片的url要用import引入，如果是字符串则不会进行解析，直接当成字符串处理

访问uv坐标
geometry.attributes.uv

## 12. 6
### uv贴图
uv贴图中的坐标和真实坐标一一对应，通过点的坐标进行关联
### 渲染管线流程
顶点数据->顶点着色器->几何着色器->图元装配->剪切->光栅化->片元着色器
1. 首先接受用户传进来的数据
2. 顶点着色器(常用的三个修饰符attribute,uniform, varying)
attribute只在顶点着色器才有，片元没有(只读,不能声明数组或结构体)
uniforms修饰的是全局变量,一般表示变换矩阵，材质，光照参数，颜色(只读，因为顶点着色器和片元着色器可以共享同一个uniform块,因此在两个着色器中,uniform声明要一致)
varying顶点着色器计算每个顶点的值写入varying变量,然后片元着色器使用该值,因此varying在顶点着色器可写,在片元着色器只读。作为顶点着色器和片元着色器之间传递插值数据,因此也要声明一致。
3. 几何着色器
几何着色器位于顶点着色和片元着色之间,根据顶点信息批量处理几何图形，因此几何着色器输入变量都是数组类型。
4. 图元装配
前面着色阶段处理的都是顶点数据,那么这些顶点是如何构成几何的信息也会被传递到OpenGL,图元装配就是将这些顶点与相关的几何图元之间组织起来，将一堆顶点数据根据原始链接关系还原出网络结构，然后准备下一步的剪裁和光栅化。
5. 剪裁
顶点可能会落在我们绘制的窗口之外,要把它和相关的图元剪切。
6. 光栅化
剪切之后马上要执行的工作就是光栅化，就是将更新后的图元传递到光栅化单元,生成对应的片元。
比如找出三角形所覆盖的像素，这时候完成坐标的转换。
7. 片元着色器
前面讲顶点着色器的输出varying,varying变量在光栅化阶段被线性插值计算后,在片元着色器作为输入
samplers: 一种特殊的uniform,用于呈现纹理...
计算顶点法向量vNormal和入射光方向两个向量的点积,计算得到的diffuse就是点光源的强度


## 12.7
### GLSL语言学习
sampler2D 2D纹理
samplerCube 盒纹理

向量访问的方式
vector.xyzw 
vector.rgba
vector.stpq

需要进行显式的类型转换

函数参数限定符
in复制到函数中可读写
out返回时从函数中复制出来
inout复制到函数中并在返回时复制出来

构造函数
聚合类型对象需要使用构造函数来进行初始化

精度限定
highp, mediump, lowp 即可完成对该变量的精度声明

invariant关键字显式要求计算结果必须精确一致
```
\#pragma STDGL invariant(all)
invariant varying  texCoord
```

限定符的优先级
invariant关键字来显式要求计算结果一致
在一般变量中  invariant > storage > precision
在参数中 storage > parameter > precision

预处理命令
```
#define 定义宏
#undef 取消已定义的宏
#if 如果给定条件
#ifdef 如果宏已经定义,则编译下面代码
#ifndef 如果宏没有定义,则编译下面代码
#elif 如果前面的#if给定条件不为真,当前条件为真,则编译一下代码
#endif 结束一个#if......#else条件编译块
```

内置的特殊变量
glsl程序使用一些特殊的内置变量与硬件沟通.他们大致分成两种,一种是input类型,他负责向硬件(渲染管线)发送数据.另一种是output类型，负责向程序回传数据，以便变成时需要。

vertex Shader中
output类型的内置变量
highp vec4 gl_Position;  (顶点坐标信息vec4)
mediump float gl_PointSize; (顶点的大小float)

fragment Shader中
input类型的内置变量 
mediump vec4 gl_FragCoord 片元在framebuffer画面的相对位置vec4
四维的最后一维是布尔值,非零代表三维空间中的一个点(x,y,z),若是0,则表示一个向量

bool gl_FrontFacing 标志当前图元是不是正面图元的一部分bool
mediump vec2 gl_PointCoord 经过插值计算后的纹理坐标 vec2

output类型的内置变量
mediump vec4 gl_FragColor 设置当前片点的颜色 vec4 RGBA color
mediump vec4 gl_FragData[n] 设置当前片点的颜色,使用glDrawBuffers数据数组 vec4 RBGA color

### 图形绘制管线的三个阶段
应用程序阶段
几何阶段
光栅阶段

1. 应用程序阶段
在该阶段的末端,几何体数据通过数据总线传送到图形硬件
2. 几何阶段
主要负责顶点坐标,光照,剪裁,投影以及屏幕映射,该阶段基于GPU进行计算
3. 基于几何阶段的输出数据,为像素正确配色,以便绘制完整图像,该阶段进行的都是单个像素的操作,每个像素的信息存储在颜色缓冲器中
光照计算涉及视点,光源,和物体的世界坐标,所以通常放在世界坐标中进行计算,而雾化以及涉及物体透明度的计算属于光栅化阶段.

## 12.8### 顶点坐标顺序
模型坐标空间(object space) -> 世界坐标空间(world space) -> 观察坐标空间(camera / eye space) -> 屏幕坐标空间(project space)
给予一个世界坐标来描述四阶矩阵控制 world matrix
观察坐标空间的生成也就是形成视锥体的过程,超出视锥体范围的东西都会被剪裁掉
观察坐标空间 -> 屏幕坐标空间总共有3步
1. 用透视变换矩阵把顶点从视锥体中变换到剪裁空间的CVV中
2. 在CVV进行图元剪裁
3. 屏幕映射: 将经过前述过程得到的坐标映射到屏幕坐标系上

图元装配(Primitive Assembly)
将顶点根据primitive(原始的连接关系)还原出网格结构。网格由顶点和索引组成,在之前的流水线中是对顶点的处理,在这个阶段是根据索引将顶点连接在一起,组成线,面单元。之后就是对超出屏幕外的三角形进行剪裁。

### 图形硬件
深度缓冲区(Z Buffer) -> 模板缓冲区(Stencil Buffer) -> 帧缓冲区(FrameBuffer) -> 颜色缓冲区(color Buffer)
Vertex program -> Fragment program 
前者的输出是后者的输入

### 矩阵
OpenGL有六种坐标
1. 物体或模型坐标系 Object or Model Coordinates
2. 世界坐标系 World Coordinates
3. 眼坐标或相机坐标 Eye or Camera Coordinates
4. 剪裁坐标系 Clip Coordinates
5. 标准设备坐标系 Normalized Devices Coordinates
6. 屏幕坐标系 Window or Screen Coordinates
坐标变换矩阵栈
vertexData -> (Object Coordinates) -> ModelViewMatrix(模型矩阵) -> (Eye Coordinates or Camera Coordinates) -> ProjectionMatrix(投影矩阵) -> (Clip Coordinates) -> Divide by w -> (Normalized Device Coordinates) -> ViewportTransform -> WindowCoordinates


### smoothstep函数
smoothstep(start,end,parameter)
```
float smoothstep(float t1, float t2, float x) {
    x = clamp((x - t1) / (t2 - t1), 0.0, 1.0);
    return x * x * (3 - 2*x);
}
```
x = start, y = 0,
x = end, y = 1,
实现更加平滑的渐变


## 12.9
### 画彩虹
```
gl_FragColor = texture2D(map, vec2(st.t, st.s));
```

### loadingmanager
其功能时处理并跟踪已加载和待处理的数据.如果为手动设置加强管理器,则会为加载器创建和使用全局实例加载器管理器
在各个不同的阶段,可以跟踪不同的数据
onStart 
onLoad
onProgress
onError

## 12.13
performance.now()的时间单位是毫秒
纹理平铺的相关属性
THREE.ClampToEdgeWrapping 表示纹理边缘与网格的边缘贴合,中间部分等比缩放。
THREE.RepeatWrapping 重复平铺
THREE.MirroredRepeatWrapping 先镜像再重复平铺

### 精灵模型模拟下雨
随机生成雨滴的位置
把所有雨滴加入group中,在render函数中改变每个雨滴的y坐标

### 帧动画模块
关键帧 KeyframeTrack
剪辑 AnimationClip
操作 AnimationAction
混合器 AnimationMixer

1. 编辑关键帧
KeyframeTrack + AnimationClip
关键帧动画
创建位置关键帧对象
```
let times = [0, 10];
let values = [0, 0, 0, 150, 0, 0]
let posTrack = new THREE.KeyframeTrack('Box.position', times, values)
```
创建颜色关键帧对象
let colorKF = new THREE.KeyframeTrack('Box.material.color', times, values)
创建关键帧数据
let scaleTrack = new THREE.KeyframeTrack('Sphere.scale', times, values)
多个帧动画作为元素创建一个剪辑clip对象,命名default,持续时间20
let clip = new THREE.AnimationClip("default", duration, [posTrack, colorKF, scaleTrack])
2. 播放关键帧
AnimationAction + AnimationMixer
```
let mixer = new THREE.AnimationMixer(group);
let AnimationAction = mixer.clipAction(clip);
通过操作Action设置播放方式
AnimationAction.timeScale = 20;
AnimationAction.play();
```
mixer.update(clock.getDelta());

## 12.14
### geometry的偏移和mesh偏移的区别
geometry的偏移是物体的网格偏移了,但是物体的世界坐标还在原来的位置,这样可以实现一个绕着世界坐标的中心点旋转的物体效果
geometry.translate(x,y,z);
mesh.rotateY(Math.PI * 0.5)
mesh的偏移是将物体的世界坐标改变,因此让其旋转,是在原地旋转
mesh.translateOnAxis(new THREE.Vector3(1,0,1), 30);

### 旋转矩阵的推导及其应用
Xnew = Xold + Tx;
Ynew = Yold + Ty;
二维坐标系下，旋转矩阵是
cos -sin
sin cos
三维坐标系下,旋转矩阵是
绕x轴旋转
1  0  0
0 cos sin
0 -sin cos
绕y轴进行旋转
cos 0 -sin
0   1  0
sin 0  cos
绕z轴进行旋转
cos sin 0
-sin cos 0
  0   0  1

### 旋转一个点
旋转轴
旋转方向
旋转角度

## 12.15
### WeakMap
属性: WeakMap.prototype.constructor
```
let wm = new WeakMap([[k1, v1], [k2, v2]])
```
WeakpMap初始化参数是一个Iterable的对象,可以是二元数组或者其他可迭代的键值对的对象。每个键值对会被加到新的WeakMap里
WeakMap对key有限制,它必须是Object

方法: 
WeakMap的方法也特别少,只有四个: delete, get, has, get
一般用于以DOM节点为键名的场景

### for in 和 for of
使用foreach遍历数组,使用break不能中断循环
for in 遍历对象 index索引为字符串型数字,不能进行几何运算
遍历顺序有可能不是按照实际数组的内部顺序
for in 会遍历数组所有的可枚举属性,包括原型。for in 遍历的是数组的索引,而for of遍历的是数组元素值
for in 更适合遍历对象, for in不适合遍历数组
for of 遍历数组的value
## 12.16
### THREE源码
Core::Object3D
属性parent和children说明,通常需要使用树来管理众多Object3D对象。
比如一辆行驶的汽车是一个Object3D对象，控制汽车行驶路线的逻辑在该对象内部实现，汽车的每个顶点经过模型矩阵的处理后，都位于正确的位置；但是汽车摆动的雨刮器，其不但随着汽车行驶方向运动，而且自身相对汽车也在左右摆动，这个摆动的逻辑无法在汽车这个对象内部的实现。解决的方法是，将雨刮器设定为汽车的chidren，雨刮器内部的逻辑只负责其相对于汽车的摆动。在这种树状结构下，一个场景Scene实际上就是最顶端的Object3D，它的模型矩阵就是视图矩阵（取决于相机）的逆矩阵。

属性matrix和matrixWorld就很好理解了，matrix表示本地的模型矩阵，仅仅表示该对象的运动，而matrixWorld则需要依次向父亲节点迭代，每一次迭代都左乘父亲对象的本地模型矩阵，直到Scene对象——当然，实际上是左乘父亲对象的全局模型矩阵。

## FGL源码阅读
### core
1. Base类
主要的存储结构是weakmap
deepObject对对象进行深度遍历把路径写入到key中,splitObject对对象进行分拆,与deepObject是一个反过程
2. SkyObject类
继承自THREE.Object3D
3. SkyBox类

### Map
1. MeshBasicEarth
INIT 
通过INIT_GEOMETRY和INIT_MATERIAL生成网格模型
并且把配置中的position和scale写入到生成的MESH中

INIT_GEOMETRY
通过配置生成对应的geometry

INIT_MATERIAL
通过配置生成对应的Material,配置中的map可能是对象,或者是一张贴图,根据map的不同类型不同处理

INITIONS
将def,opt合并,形成新的配置参数

setEnvMap
设置环境地图

dispose
删除地图上对应的Mesh实例

2. GeojsonPointMap
与上面类似，不同的是需要遍历json文件来生成球状地图
多了一个CREATE_POINT方法,根据json文件的数据来生成对应的球面坐标

3. GeojsonLineMap
因为是线状图,所以首先就有CREATE_LINE这样的一个方法,循环coordinates数组,数组中的前后两项构成线
同时在INIT_GEOMETRY中添加自定义属性
在fs文件中处理动画相关的亮度
setMaterialUniform

4. GeojsonInteractionMap
INIT_GEOMETRY
生成PlaneBufferGeometry,
并将几何体平移到中心点

5. MyFamilyBuckets 
INIT 
循环initions创建动画,将动画的id加入到map中



## 12.22
vuepress插件
文档实例的最佳实践方案
Demo Container使用Vuepress的chainMarkdown,extendMarkdown API 拓展了其内部的markdown对象

### vuepress-plugin-demo-block
通过Vuepress clientRootMixin API混入页面的mounted,updated生命周期,读取示例代码分离template,script,style代码块
template包裹的代码块直接插入示例节点
script包裹的代码块通过Vue.extend编译出Vue对象,再调用其$mount()方法挂载到示例dom
style包裹的代码块直接插入document
这么做的问题是template代码块中不能包含Vuepress中全局注册的组件

### vuepress-plugin-demo-code
通过Vuepress extendMarkdown API 拓展内部markdown对象,进而识别::: demo xxx :::代码块,将其包裹的示例代码直接插入Markdown文档等待vue-loader处理。

### vuepress-plugin-extract-code
提供了一个RecoDemo组件用于在Markdown中构造示例页面,并通过Vuepress chainMarkdown API给Vuepress内部的markdown添加一个插件,该插件负责,手动解析RecoDemo中的<<< @/docs/.vuepress/demo/demo.vue?template语法

### 插件配置
component
类型: string
默认值: demo-block
包裹代码与示例的组件名称
通过Slot demo(被渲染成示例), Slot description(被渲染成示例描述信息), Slot source(被渲染成示例的源代码)

## 12.23
### Reflect.ownKeys()
返回一个由目标对象自身的属性键的数组
### Map和Object的区别
Object本质上是哈希结构的键值对的集合,它只能用字符串,数字或者Symbol等简单数据类型当作
将dom节点作为键,但是由于对象只接受字符串作为键名,所以键被自动转为字符串[object HTMLDivElement]
Map类继承了Object,并对Object功能做了一些拓展,Map的键可以是任意的数据类型

同名碰撞
对象其实就是在堆开辟了一块内存,其实Map的键存的就是这块内存的地址。只要地址不一样,就是两个不同的键,这就解决了同名属性的碰撞问题,object属性名相同的话,就只能覆盖先前的值

可迭代
Map实现了迭代器,可以同for...of遍历,而Object不行

长度
Map可以直接拿到长度,而Object不行

有序性
填入Map的元素,会保持原有的顺序,而Object无法做到。

可展开
Map可以使用省略号展开,而Object不行。

### 不可迭代对象变成可迭代对象(Symbol.iterator)
设计一个迭代器
迭代器本身是一个对象,这个对象有next()方法返回结果对象,这个结果对象有下一个返回值value,迭代器完成布尔值done,
```
function createIterator(items){
    let i = 0;
    return{
        next(){
            let done = (i>=items.length);
            let value = !done ? items[i++] : undefined;
            return{
                done,
                value
            }
        }
    }
}
let arrayIterator = createIterator([1,2,3])
```
创建迭代器
ES6封装了generator用来创建迭代器。显然生成器是返回迭代器的函数,这个函数通过function后的*号表示,并使用新的内部专用关键字yield指定迭代器next()方法的值
用ES6生成器创建一个迭代器
```
let obj = {
    *createIterator(items){
    for(let i in items){
        yield items[i];
    }
  }
}
```
let someIterator = createIterator([123, 'mge']);
可迭代对象具有Symbol.iterator属性,即具有Symbol.iterator属性的对象都有的默认迭代器
判断是否可迭代
```
const isIterator = obj => obj != null && typeof obj[Symbol.iterator] === 'function';
```

## 12.24
### LatheGeometry
旋转造型,可以利用已有的二维数据生成三维顶点,二位数据可以通过二维向量对象Vector2定义,默认绕y轴旋转
样条曲线插值计算
借助Shape对象的方法.splineThru,把上面的三个顶点进行样条插值计算,可以得到一个光滑的旋转曲面
```
var shape = new THREE.Shape();//创建Shape对象
var points = [...vector2]
shape.splineThru(points);
var splinePoints = shape.getPoints(20); //二维形状
var geometry = new THREE.LatheGeometry(splinePoints,30); //旋转
```

## 12.27
设置index的时候节点按逆时针顺序连接,顺序不对就看不见了
shapeGeometry顶点顺序和加入时相反,shape.arc的圆心是相对于上一个节点的相对坐标
glsl mix(x,y,a)
a控制混合结果 x(1-a)+y*a

Material的公有属性
webgl深度缓冲

## 12.28
### OpenGL的空间变换
#### 世界空间
相对于其他空间来说是不变的，所以,它也被用作空间变换的参考系
#### 模型空间
可以以世界坐标系中的某点来作为模型空间的原点
模型变换(模型-世界变换)
默认情况下模型坐标系的原点位于世界坐标系的原点,我们可以通过一系列缩放,旋转,平移,将模型以任意角度摆在任意位置。
这种情况下,模型的顶点以及模型自身的坐标系都会相对世界坐标系变化。

假设有一个模型坐标系表示为矩阵 M（基于世界坐标系来描述），一个顶点在该模型坐标系上的坐标表示为列向量 D。 那么，该顶点在世界坐标系中的坐标 D‘，有如下变换关系：M·D = D’。M 也称为模型矩阵。模型矩阵本质上是一系列缩放、旋转和*移矩阵的复合矩阵。
#### 视图空间
是以摄像机的角度来定义的一个空间。
正交投影中,摄像机与视图坐标系的位置关系图类似于模型-世界坐标系
世界坐标系是父坐标系

视图变换的实质就是将某个顶点在世界空间中的描述，转换为在视图空间中的描述。假设有一个顶点在在世界坐标系中的坐标表示为列向量 D‘，一个视图坐标系表示为矩阵 V（基于世界坐标系来描述的），那么该顶点在视图坐标系 V 中的坐标 D，有如下变换关系：V·D = D’（道理和模型变换类似）。设 V 的逆矩阵为 V’，可以推导出变换关系 V‘·D‘ = D，V’ 也被我们称为视图矩阵。

采用模型视图矩阵的优点
渲染一个模型时，我们通常需要将模型坐标转换成世界坐标，世界坐标转成视图坐标
将模型矩阵和视图矩阵结合在一起就形成了模型视图矩阵
1. 一次矩阵变换比两次矩阵变换更高效
2. 一次矩阵变换的精确性更高

透视投影的剪裁规则
在变换到裁剪空间之后，我们将赋予齐次坐标的 w 分量更加丰富的含义：作为一个临界值来判断一个经过裁剪变换后的顶点是否位于景视体内。如果变换后的坐标值 x、y、z 均在区间 [-w, w] 内，则表明该顶点在视景体内。否则，表明该顶点不在视景体内，将会被抛弃。

正交投影的剪裁规则
如果变换后的坐标值 x、y 、z 均在区间 [-1, 1] 内，则表明该顶点在视景体内。而且这里的 w 为 1，所以其裁剪的判断规则与透视投影中是一致的。

### 标准设备坐标空间（NDC Space）
对于正交投影，任意顶点在裁剪空间的坐标值 x、y 、z 均在区间 [-1, 1] 内，这种情况下无需任何变换，裁剪空间本身也是标准设备坐标空间。

对于透视投影，我们只需要对顶点在裁剪空间的坐标执行齐次坐标标准化，使其 w 分量变为 1。对应的 x、y、z 也将会缩小到范围 [-1, 1] 内。这种情况下，标准化的过程其实也是将顶点从裁剪空间坐标变换到到标准设备坐标空间的过程。

## 12.30
### PlaneGeometry的绘制顺序 右下 -> 左下 -> 右上 -> 左上 

## 1.5
### this.$router和this.$route的区别
this.$router相当与全局的路由器对象,包含了很多属性和对象,任何页面都可以调用其push(), replace(), go() 等方法
this.$route表示当前路由对象,每一个路由都会有一个route对象,是一个局部对象,可以获取对应的name,path,params,query等属性

### v-for和v-if优先级的高低
v-for优先级更高

### import
import * as xxx from 
这种导入是把所有的输出包裹到对象中

es6 重命名 当引入的两个模块变量名相同时，可以使用重命名的方式

## 1.6
### 本地测试npm包
npm link 可以在本地项目和本地npm模块之间建立连接,进入包内 npm link,包会被映射到全局中的node_global -> node_modules中
进入本地项目中, npm link 模块名 (package.json中的name)

## 1.18
### THREE新版本和旧版本orbitControls的区别
新版本 1.35
旧版本 0.92

旧版本使用需要使用引入orbitControls作为以来
新版本则是直接导出构造函数

旧版本还需要每一帧更新control
```
this.$controls.update();
this.$renderer.render(this.$scene, this.$camera);
```

## 1.20
### Material 学习
基于深度着色的MeshDepthMaterial
可以通过相机的near和far参数来控制物体是否可见
改变场景中的所有的物体的材质
```
var scene = new THREE.Scene();
scene.overrideMaterial = new THREE.MeshDepthMaterial();
```
#### blending属性
NoBlending:z-buffer值较大的像素将会遮挡z-buffer值较小的像素,没有纹理融合的效果,设置纹理透明度无效
NormalBlending:默认选项,根据z-buffer正常显示纹理,这是标准混合模式,它单独使用顶层,而不将其颜色与其下面的层混合
AdditiveBlending:此混合模式只是将一个图层的像素值添加到另一个图层,如果值大于1，则显示白色。线性减淡颜色值,由于它总是产生与输入相同或更浅的颜色，因此它也被称为"加亮"
SubtractiveBlending:此混合模式将一个图层的像素值减去另一个图层像素值，如果为负数，则显示黑色。
MultiplyBlending:颜色混合,源图像的RGB分量与目标图像RGB分量的相乘。

#### 深度缓冲区
深度测试的意义在于舍弃片元与否。
深度写入的意义在于深度测试的基础上，要不要覆盖深度缓冲，即重新设立深度测试的标准。
深度写入可以保证像素级的深度关系
GPU的渲染流程

片元 -> 模板测试 -> 深度测试 -> 混合 -> 颜色缓冲区

blending: THREE.AdditiveBlending 把源和目标的RGB三通道分别进行相加
混合设置要传给WebGLRenderer渲染材质的RGB和透明度, 所以材质参数中要开启透明度设置

深度测试的大概流程：

开始深度测试 -> 是否开启了深度测试 -> 比较该片元的深度和已经存在于深度缓冲区中的深度值(如果当前 fragment 的 Z 值小于取出来的值则更新 depth buffer 里面的值, 如果大于取出来的值说明该点在一个物体后面这个 fragment 将会被丢弃) -> 是否通过了深度测试 -> 是否开启了深度写入 -> 将深度值写入深度缓冲区

混合的大概流程: 

开始混合 -> 是否开启了混合 -> 得到片元的颜色值 和 已经存在与颜色缓冲区的颜色值 -> 进行混合操作 -> 更新颜色缓冲区的值

AlphaTest(透明度测试)

仅渲染在一定范围内的像素,和深度测试一样,也可以定义比较的规则


半透明模型和不透明模型
在进行不透明物体渲染时,我们需要开启深度缓冲区,进行深度写入操作,保证场景中的不透明物体间有正常的深度关系。
半透明模型需要需要遵循画家算法由远及近进行绘制,最终效果才会正确

## 1.24

### z-fighting
两个图形在同一个像素上的深度相同而出现闪烁
解决z-fighting的思路
1.设置不同的深度
2.设置合适的near和far值
near和far与深度缓冲也密切相关,深度缓冲其实是非线性的,靠近相机的地方精度越高,

### 四元数
q = ((x,y,z), w) 


## 1.29
### 矩阵变换
Object3D的3种矩阵对象
Object3D.matrix 相对于其父对象的局部模型变换矩阵
Object3D.matrixWorld 对象的全局模型变换矩阵
Object3D.modelViewMatrix 表示对象相对于相机坐标系的变换,


## 2.8
### 齐次坐标
点位置: (x,y,z,w) w不为0
向量: (x,y,z,0)
用矩阵来表示三维空间中的对象
### 四元数
表达式(a, b, c, d)
a = sin(theta / 2) * A.x
b = sin(theta / 2) * A.y
c = sin(theta / 2) * A.z
d = cos(theta / 2)


## 2.10
### 屏幕射线的计算
1. 首先确定相机的原点
2. 随后计算点击像素点坐标在world frame中的坐标
3. 最后,在world frame中使用点击的像素点的世界坐标减去相机位置,标准化后得到方向矢量
   
### 射线和几何体相交检测
射线和几何体相交检测实际上是要计算出屏幕射线和构成几何体的所有三角形是否相交,只要射线和其中至少一个三角形相交，我们就认为射线和几何体相交。
通常使用绑定容积进行保守检测计算。 bounding volume通常为围绕几何体的球体胡后者方形体, 这些sphere或者box一般根据几何体的顶点数据近似获得。

使用几何体的顶点表达来获得Bounding Volume, 不管是sphere还是box,比较容易确定。

ray-geometry相交检测的原理
屏幕射线和几何体的多个面中有一个相交就相交

## 2.17
### 齐次坐标与仿射变换
在三维空间一个点(x, y, z)可以在齐次坐标中用一族来表示
仿射变换是两种简单变换的叠加，一个是线性变换， 一个是平移变换

### console.log() 的延迟打印问题
chrome的控制台出于性能考虑,对引用数据类型的数据读取是存在延迟的,默认读取一层数据,当你点击展开时，会重新去堆内存中读取属性值和下一层的数据

