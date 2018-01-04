# Matrix  
## Matrix简介  
> Matrix是一个矩阵，主要用来做坐标映射，数值转换  
  
Matrix的样子：  
```  
|MSCALE_X	MSKEW_X		MTRANS_X|  
|MSKEW_Y	MSCALE_Y	MTRANS_Y|  
|MPERSP_0	MPERSP_1	MPERSP_2|  
```  
**(MPERSP_0, MPERSP_1, MPERSP_2)三个参数是控制透视的，在3D效果中运用，通常为(0, 0, 1)**  

### Matrix实现坐标变换的原理  
> 这里的点都是使用齐次坐标系来表示的，(x, y, 1)表示点；(x, y, 0)表示向量  
  
1. 缩放  
变换矩阵：  
```  
|x'|   |MSCALE_X	0		0| |x|  
|y'| = |0		MSCALE_Y	0| |y|  
|1|    |0		0		1| |1|  
```  
结果：  
x' = MSCALE_X\*x + 0\*y + 0\*1 = MSCALE_X\*x  
y' = 0\*x + MSCALE_Y\*y + 0\*1 = MSCALE_Y\*y  
2. 错切  
水平错切，变换矩阵：  
```  
|x'|   |1	MSKEW_X	0| |x|  
|y'| = |0	1	0| |y|  
|1|    |0	0	1| |1|  
```  
结果：  
x' = x + MSKEW_X\*y + 0\*1 = x + MSKEW_X\*y  
y' = 0\*x + 1\*y + 0\*1 = y  
垂直错切，变换矩阵：  
```  
|x'|   |1	0	0| |x|  
|y'| = |MSKEW_Y	1	0| |y|  
|1|    |0	0	1| |1|  
```  
结果：  
x' = x + 0\*y + 0\*1 = x  
y' = MSKEW_Y\*x + 1\*y + 0\*1 = MSKEW_Y\*x + y  
复合错切，变换矩阵：  
```  
|x'|   |1	MSKEW_X	0| |x|  
|y'| = |MSKEW_Y	1	0| |y|  
|1|    |0	0	1| |1|  
```  
结果：  
x' = x + MSKEW_X\*y + 0\*1 = x + MSKEW_X\*y  
y' = MSKEW_Y\*x + 1\*y + 0\*1 = MSKEW_Y\*x + y  
3. 旋转  
有一点(x, y),其在坐标中的位置可以表示为：  
```  
x = rcosα  
y = rsinα  
```  
(x, y)绕原点旋转θ弧度，旋转得到的点的坐标：  
```  
x' = rcos(α + θ) = rcosαcosθ - rsinαsinθ = xcosθ - ysinθ  
y' = rsin(α + θ) = rsinαcosθ + rcosαsinθ = ycosθ + xsinθ  
```  
矩阵表示：  
```  
|x'|   |cosθ	-sinθ	0| |x|  
|y'| = |sinθ	cosθ	0| |y|  
|1|    |0	0	1| |1|  
```  
4. 平移  
变换矩阵：  
```  
|x'|   |1	0	x1| |x|  
|y'| = |0	1	y1| |y|  
|1|    |0	0	 1| |1|  
```  
结果：  
x' = 1\*x + 0\*y + x1\*1 = x + x1  
y' = 0\*x + 1\*y + y1\*1 = y + y1  
### Matrix复合变换原理  
> 我们能使用的四种变换，所对应的matrix的操作都有三类：前乘(pre), 后乘(post), 设置(set)  
  
对应的计算：  
```  
前乘：M' = M\*X  
后乘：M' = X\*M  
```  
复合变换基本定理：  
* 所有的变换(平移，旋转，错切，缩放)都是以**当前的坐标原点**为基准点进行变换的  
* 在每次进行变换后，坐标系相关（原点，x轴方向，y轴方向）也相应变换，所以会影响后续变换（就是上述的后续的每次变换的基准点就发生了变换）  

**在进行复杂的复合变换时，可以先进行矩阵的推算，得出变换矩阵的计算公式（如：S\*M\*R\*T），再根据矩阵计算公式，得出应该使用前乘(pre)还是后乘(post)**  
**反之，根据程序代码，要得出最终变换结果，须进行的分析是：程序永远是从上往下，一行一行的执行的，那么为什么使用前乘和使用后乘，有时会导致变换结果不一致呢，首先，矩阵是不满足交换律的，其次就是上面两条定理的影响了，当我们使用前乘(pre)时，当前的坐标系使用的是变换后的Matrix的坐标系，而使用后乘(post)时，当前的坐标系是这个变换矩阵（比如，旋转45度，系统构造出来的矩阵）的坐标系，这个坐标系就是默认的坐标系，也就是原点在屏幕左上方的坐标系，清楚每次变换的坐标系之后，那么此次变换的结果也就清楚了**  
## Matrix方法  
### 数值计算  
> mapPoints, mapRadius, mapRect, mapVectors:计算经过变换之后的点，平均半径，矩形，向量  
  
### 特殊方法  
#### setPolyToPoly  
> setPolyToPoly(float[] src, int srcIndex, float[] dst, int dstIndex, int pointCount)   
  
方法的作用：Set the matrix such that the specified src points would map to the specified dst points.（真是不知道怎么说呀）  
就是把src中pointCount个点绘制到dst中pointCount个点对应的位置，pointCount取值是0， 1， 2， 3， 4  
#### setRectToRect  
> setRectToRect(RectF src, RectF dst, ScaleToFit stf)  
  
方法的作用：将源矩阵的内容绘制到目标矩阵区域  
* 当stf=ScaleToFit.START/CENTER/END时，如果源矩阵的大小不目标矩阵的大小不相等，绘制的时候会对源矩阵中的内容进行缩放，缩放的规则是：**让源矩阵的内容的最大边与目标矩阵的最小边相等为止**  
* 当stf=ScaleToFit.FILL时，如果源矩阵的大小不目标矩阵的大小不相等，绘制的时候会对源矩阵中的内容进行缩放，让绘制内容填满目标矩阵  
## Matrix Camera  
> 位于包graphic中，主要是给View拍照的相机，作用是将3D内容拍扁成为2D的内容  
  
### Camera常用方法  
* save,restore:保存，回滚  
* getMatrix,applyToCanvas:获取Matrix，应用到画布  
* translate:位移  
* rotateX,rotateY,rotateZ:围绕不同坐标轴旋转  
* setLocation,getLocationX,getLocationY,getLocationZ:设置与获取相机位置  
### Camera坐标系  
> Camera使用的坐标系是左手坐标系：左手手臂指向x轴的正方向，四指弯曲指向y轴正方向，大拇指指向z轴正方向，默认原点位置：屏幕左上角  
  
要将三维空间的内容显示在二维平面上需要使用三维投影，三维投影的分类：  
* 正交投影：如正视图，侧视图，仰视图，俯视图等  
* 透视投影：遵循近大远小的关系，这里的Camera使用的就是透视投影  
Android上面观察View的摄像机默认位置在屏幕的左上角，且距离屏幕有一段距离  
### Camera的平移及旋转变换  
> 记住这里的平移和旋转变换虽然是Camera上的方法，但是我们最后都是应用到了绘制内容的画布上，所以使用了变换之后，实际上Camera的位置是不会发生变化的，依然在(0, 0, -8)坐标上，所以实际上发生变换的一定是绘制的View  
  
测试代码如下：  
```  
	camera.translate(0, 0, increaseZ ? 1 : -1);
        Matrix matrix = new Matrix();
        camera.getMatrix(matrix);
        canvas.drawBitmap(icon, matrix, paint);
        postInvalidate();  
```  
或：  
```  
	camera.translate(0, 0, increaseZ ? 1 : -1);
        camera.applyToCanvas(canvas);
        canvas.drawBitmap(icon, 0, 0, paint);
        postInvalidate();  
```  
通过上述代码，可以有以下几点结论：  
* Camera上的变换最终是应用到了绘制的View上  
* Camera的位置不会发生变化，依然是在(0, 0, -8)坐标上（但这个"-8"的单位貌似不是像素！！测试了下，这个-8对应的确实是-576像素，也就是-8\*72）  
* 当绘制的内容的z轴坐标小于Camera的z坐标值（也就是不在Camera的z轴正方向），那么我们就看不见这个View了  
* 由上述所说的，由于Camera的位置是不变的，所以，我们在借助Camera进行一些View的变换时，需要考虑Camera的位置以及其所在的坐标系原点（比如使用了Camera的rotate相关方法，Camera对应的坐标原点还在屏幕的左上角，但是对应的x,y,z轴的方向相对于屏幕就发生了变换了）  
