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














