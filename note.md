## 11.24
### webpack配置路径别名
webpack.base.conf.js
resolve -> alias -> 配置项
例子：
```
alias: {
    'vue$': 'vue/dist/vue.esm.js',
    '@': resovle('src'),
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
四维的最后一维是布尔值,1代表三维空间中的一个点(x,y,z),若是0,则表示一个向量

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

## 12.8
### 顶点坐标顺序
模型坐标空间(object space) -> 世界坐标空间(world space) -> 观察坐标空间(eye space) -> 屏幕坐标空间(project space)
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
vertexData -> (Object Coordinates) -> ModelViewMatrix(模型矩阵) -> (Eye Coordinates) -> ProjectionMatrix(投影矩阵) -> (Clip Coordinates) -> Divide by w
->(Normalized Device Coordinates) -> ViewportTransform -> WindowCoordinates
ViewMatrix(视图矩阵)


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

地球