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
|x'|   |1	0		x1| |x|  
|y'| = |0		1	y1| |y|  
|1|    |0		0		1| |1|  
```  
结果：  
x' = 1\*x + 0\*y + x1\*1 = x + x1  
y' = 0\*x + 1\*y + y1\*1 = y + y1  
  