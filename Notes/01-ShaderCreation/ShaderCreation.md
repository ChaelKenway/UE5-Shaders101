Main Link:

[UE4 Material Editor - Shader Creation](https://www.youtube.com/playlist?list=PL78XDi0TS4lFlOVKsNC6LR4sCQhetKJqs)

# 04 Distortion Shader

[Link](https://www.youtube.com/watch?v=gwx2NOZJ5CE&list=PL78XDi0TS4lFlOVKsNC6LR4sCQhetKJqs&index=4)

Extra videos:

[01](https://www.youtube.com/watch?v=yj7-h_X_eu4)

[02](https://www.youtube.com/watch?v=Q2UY8Pxl7kY)

[03](https://www.youtube.com/watch?v=sR0xV0TDvS8)

Extra resources:

[Unreal4 UV Tricks](http://www.tharlevfx.com/unreal-4-uv-tricks)

## Move Textures

TexCoord + Vec2

TexCoord + Time * Vec2

TexCoord + (Noise.x, Noise.y) * Intensity 实现材质扭曲

可以先对noise进行偏移，再通过Texcoord加到纹理上。

![0104-1](0104-1.png)

# 05 Flipbook Animation

[Link](https://www.youtube.com/watch?v=ZWAF_f2aP9s&list=PL78XDi0TS4lFlOVKsNC6LR4sCQhetKJqs&index=5)

![0105-1](0105-1.png)

# 06 Environment Blending

[Link](https://www.youtube.com/watch?v=QwVDlS8uiYg&list=PL78XDi0TS4lFlOVKsNC6LR4sCQhetKJqs&index=6)

使用世界空间法线B通道，在模型的上方混合一个新的材质。

从法线图中提取向上方向，可以更精确地进行混合。这里需要将法线的切线空间转换到世界空间。

切线空间：UVW。UV即纹理的UV，W为垂直UV远离模型表面的方向。[Wiki](https://en.wikipedia.org/wiki/Tangent_space_)

世界空间：世界坐标的XYZ。Z为绝对的向上方向。

[UE文档，Shader向量操作](https://docs.unrealengine.com/5.1/en-US/vector-operation-material-expressions-in-unreal-engine/#transform)

![0106-1](0106-1.png)

来自UE文档，将切线空间转换到查看空间(view space)制作Billboard效果。

![0106-2](0106-2.png)

# 07 Shader Performance Optimization

[Link](https://youtu.be/D8E47BJOE6E?list=PL78XDi0TS4lFlOVKsNC6LR4sCQhetKJqs)

## 查看性能

### alt + 8

查看着色器复杂度，但只是一个概括性的表示。

### Shader instruction count

Shader Graph -> HLSL -> Assembly Instructions -> Graphics Driver

缺陷：

* 并非所有的指令消耗同样的时间。

* 不同平台需要不同的指令数量。

### Test on your target platform

实机测试，最准确的方式。

## 优化性能

### 删掉不要的东西

### 重构数学表达式

![0107-1](0107-1.png)

### 使用四维向量来减少重复的节点

![0107-2](0107-2.png)

### 将多张纹理合成在一张纹理中

比如Substance Painter的ORM
 
# 08 Bump Offset and Parallax Occlusion Mapping

[Link](https://www.youtube.com/watch?v=wc0StMr3CQo&list=PL78XDi0TS4lFlOVKsNC6LR4sCQhetKJqs&index=8)

## BumpOffset

输入一张高度图，再对原有纹理采样。

![0108-1](0108-1.png)

## Parallax Occlusion Mapping

输入纹理对象，高度比率，参考平面。节点采用射线对纹理进行采样。需要消耗更多的性能。

修改Min/Max Steps改变高度的精度。

# 09 Texture Compression and Settings

[Link](https://www.youtube.com/watch?v=h95X255NhOo&list=PL78XDi0TS4lFlOVKsNC6LR4sCQhetKJqs&index=9)

[DXT参考资料](http://wiki.polycount.com/wiki/DXT)

DXT1能使容量减少到1/8，DXT5能使容量减少到1/4。

Aplha压缩：只会使用Alpha通道。

如果不需要使用颜色，关闭sRGB。

# 10 Cloth Shader

[Link](https://www.youtube.com/watch?v=LUfG5ojx0O4&list=PL78XDi0TS4lFlOVKsNC6LR4sCQhetKJqs&index=10)

布料特点：

1. 针织布料的边缘有些许突起的纤维，使得有亮光

![0110-1](0110-1.png)

2. Satin（缎子）相反，边缘更暗。直面的区域更亮。

![0110-2](0110-2.png)

3. 丝绸同理，面向光源或摄像机的表面更亮。

[神秘海域4的布料效果](https://www.artstation.com/artwork/6kEmV)

[Siggraph的ppt](http://advances.realtimerendering.com/s2010/Hable-Uncharted2(SIGGRAPH%202010%20Advanced%20RealTime%20Rendering%20Course).pdf)，布料算法在第80页。

菲涅尔效果：相机向量点乘像素法线。像素法线可以用法线图（需要transform）。

使用菲涅尔+反菲涅尔改变边缘或中间的亮度。

![0110-3](0110-3.png)

# 11 Volumetric Ice Shader

[Link](https://www.youtube.com/watch?v=X5WASspig3g&list=PL78XDi0TS4lFlOVKsNC6LR4sCQhetKJqs&index=11)

Other Resources: 

[1](https://www.artstation.com/blogs/kanoba/Ajpj/paragon-breakdown-001-ice-shader-part-i)

[2](https://80.lv/articles/developing-artistic-ice-in-unreal-engine-4/)

实现“两层”的效果：冰的表面与下面。并且“下面”位置会随着摄像机角度变化而变化。

![0111-1](0111-1.png)

由CameraVector得到ReflectionVector，将其反向得到TransmissionVector，用来驱动UVcoord。

![0111-2](0111-2.png)

计算反射向量

![0111-3](0111-3.png)

处理反射向量。用其B和材质的分辨率控制控制纹理向内凹陷的深度。

![0111-4](0111-4.png)

将(0, 0, 1)替换成一张法线图，可以在表面做出凹凸的效果。其次，用一张灰度图替换Depth，可以使得不同地方的凹陷程度不同。

![0111-5](0111-5.png)

# 12 Rustling Leaves Shader 

[Link](https://www.youtube.com/watch?v=5FxaVC5w97w&list=PL78XDi0TS4lFlOVKsNC6LR4sCQhetKJqs&index=12)

使用sine+TexCoord制作扰动效果，和一张蒙版相乘，使得不同绘制区域的树叶，运动模式不同。

同时sine+Normal使得法线产生相对应扰动效果，更具有3D感。

![0112-1](0112-1.png)

# 13 Rain Wetness Shader

[Link](https://www.youtube.com/watch?v=fYGOZYST-oQ&list=PL78XDi0TS4lFlOVKsNC6LR4sCQhetKJqs&index=13)

效果：沾水的表面会变暗，更加饱和（给Desaturation传入一个负数），粗糙度大幅降低。

这里使用了材质函数。输入原材质的BaseColor, Roughness, Specular。

补充一个水面的Mask，以及“渗透性”（Porousness，只影响颜色，值越高，色彩越潮湿）。这里将Roughness和Specular组成了Vector2进行运算。

![0113-1](0113-1.png)

# 14 Rain Drops Shader

[Link](https://www.youtube.com/watch?v=5eyq2FJ6lig&list=PL78XDi0TS4lFlOVKsNC6LR4sCQhetKJqs&index=14)

[制作雨滴法线图](https://www.youtube.com/watch?v=YbiX2FhmlaU)

Other Resources

[1](https://www.youtube.com/watch?v=bXEzhntMnwc)

[2](https://www.youtube.com/watch?v=ymAuk1z6f-g)

雨滴效果思路：

1. 通过改变法线（改变RG，B恒定为1）体现出雨滴。静态雨滴法线恒定，动态雨滴处的法线强度在0-1中循环。

2. 只有模型的上方才会有雨滴，使用VertexNormal(B)。

3. 雨滴的粗糙度非常低，反转雨滴法线的RG，提取出雨滴的区域。

![0114-1](0114-1.png)

# 15 Rain Drip Shader

[Link](https://www.youtube.com/watch?v=rhlV4okjkiA&list=PL78XDi0TS4lFlOVKsNC6LR4sCQhetKJqs&index=15)

效果思路：

1. 提取出模型的侧面。

2. 使用灰度不同的竖条，改变不同位置的流水速度。

3. 使用面条状的流线，制作流水形状。

4. 用很扁的长条做蒙版，乘第3步的流水。

5. 水流处法线和粗糙度处理与14同理。

![0115-1](0115-1.png)

# 16 Rain Ripples Shader

[Link](https://www.youtube.com/watch?v=ABWzKYc6UQ0&list=PL78XDi0TS4lFlOVKsNC6LR4sCQhetKJqs&index=16)

Time->Frac制作从0到1不断变化的小数，-1+圆形渐变，制作向外扩散的效果。放大后传入sine，可以得到一层一层的涟漪效果。

下图中纹理的R通道储存了多个圆形渐变；A是灰度不同的圆形，使得不同位置的涟漪交替出现；GB存储了法线的XY。

![0116-1](0116-1.png)

多个涟漪可以交叉叠加，所以使用不同的时间，不同的UV，不同的强度传入上图的函数，再将4个法线叠加。

![0116-2](0116-2.png)

叠加函数如下。先提取所有法线的RG，乘强度后相加；提取所有法线的B，做插值处理lerp(1, B, 强度），再全部相乘。最后将处理过的RG和B组合得到输出法线。

![0116-3](0116-3.png)

# 17 Rain Puddle Shader 

[Link](https://www.youtube.com/watch?v=7COoNjAGmAk&list=PL78XDi0TS4lFlOVKsNC6LR4sCQhetKJqs&index=17)

17就是对16的效果制作遮罩，确定涟漪产生的位置；同时在涟漪上叠加一层水面（两个水面法线叠加）的效果。

使用一张噪波，提取出需要产生水坑的地方；同时确保效果只产生在模型上方；有水坑的地方，粗糙度大幅降低。

![0117-1](0117-1.png)

水面效果很简单，叠加两张水的法线。

叠加法线的技巧：两张纹理的RG叠加，乘强度；各自的B直接相乘；最后append。

![0117-2](0117-2.png)

# 18 Complete Rain Shader

[Link](https://www.youtube.com/watch?v=tUemJLSaFxw&list=PL78XDi0TS4lFlOVKsNC6LR4sCQhetKJqs&index=18)

Other Resources:

[1](https://www.fxguide.com/fxfeatured/game-environments-partc/)

[2](https://80.lv/articles/setting-up-rain-materials-in-substance-designer/)

[3](https://seblagarde.wordpress.com/2013/01/03/water-drop-2b-dynamic-rain-and-its-effects/)

13~17的效果集合。

13——潮湿效果，14——雨滴，15——下雨流水，16——涟漪，17——水坑。

Wetness 潮湿

![0118-Wetness](0118-Wetness.png)

RainDrop 雨滴

![0118-RainDrop](0118-RainDrop.png)

RainDrip 流水

![0118-RainDrip](0118-RainDrip.png)

RainPuddle 水坑（包含了涟漪和水面）

![0118-RainPuddle](0118-RainPuddle.png)

Rain 总效果

* Rain控制下雨程度，影响雨滴和流水。

* Wet控制整体的潮湿程度。

* Puddles, Wind, Ripple控制水坑，水面，涟漪。

* WetRoughness控制潮湿处的粗糙度。

* 输入原材质的BaseColor, Specular, Roughness, Normal。

![0118-Rain](0118-Rain.png)

# 19 Procedural Noise

[Link](https://www.youtube.com/watch?v=QIlxWkfZK-8&list=PL78XDi0TS4lFlOVKsNC6LR4sCQhetKJqs&index=19)

程序化噪点

1. 函数：常用快速梯度-3D纹理。

2. 关卡：数量越高，细节越多，性能消耗越大。

3. 最小/最大输出：图像灰度的值。

# 20 Snowy Tree Trunk Shader

[Link](https://www.youtube.com/watch?v=dco2ra0xAcY&list=PL78XDi0TS4lFlOVKsNC6LR4sCQhetKJqs&index=20)

使用noise在树皮材质的基础上添加上雪材质。noise输入世界坐标，使用3D纹理。

![0120-1](0120-1.png)

# 21 Rock Layers Shader 

[Link](https://www.youtube.com/watch?v=cGmWa-gETzU&list=PL78XDi0TS4lFlOVKsNC6LR4sCQhetKJqs&index=21)

Other Resources

[1](https://www.youtube.com/watch?v=Ymo-izZN6UI)

[2](https://www.youtube.com/watch?v=kc76I0iSUck)

使用世界坐标B使得石头在世界空间的Z轴上始终保持分层的效果；加上噪波，使得每一层之间有随机的过渡效果。

![0121-1](0121-1.png)

# 22 World-Aligned Textures 

[Link](https://www.youtube.com/watch?v=pXOknekvmwE&list=PL78XDi0TS4lFlOVKsNC6LR4sCQhetKJqs&index=22)

WorldAlignedTexutre节点能将纹理3D映射到XYZ平面上，不考虑模型本身的UV。

![0122-1](0122-1.png)

# 23 Water Ripples Shader

[Link](https://www.youtube.com/watch?v=r68DnTMeFFQ&list=PL78XDi0TS4lFlOVKsNC6LR4sCQhetKJqs&index=23)

水的特征：

1. 波纹。

2. 随深度改变颜色，浅水几乎是透明的。

3. 反射。垂直看向水面时，能看到底部；几乎平行看时，反射很明亮。

4. look through水时，波纹和涟漪使得看到的水下的物体产生扭曲形状。

关键点：

* Surface Ripples [x]

* Depth Color Gradient [ ]

* Depth Opacity [ ]

* Reflection [ ]

* Refraction [ ]

将两张水面法线进行平移并叠加。另外再叠加一张大的法线。最后还可以加上下雨效果。

![0123-1](0123-1.png)
















