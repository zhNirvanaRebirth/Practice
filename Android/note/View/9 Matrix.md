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
 