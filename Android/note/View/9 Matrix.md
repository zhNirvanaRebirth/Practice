# Matrix  
## Matrix���  
> Matrix��һ��������Ҫ����������ӳ�䣬��ֵת��  
  
Matrix�����ӣ�  
```  
|MSCALE_X	MSKEW_X		MTRANS_X|  
|MSKEW_Y	MSCALE_Y	MTRANS_Y|  
|MPERSP_0	MPERSP_1	MPERSP_2|  
```  
**(MPERSP_0, MPERSP_1, MPERSP_2)���������ǿ���͸�ӵģ���3DЧ�������ã�ͨ��Ϊ(0, 0, 1)**  

### Matrixʵ������任��ԭ��  
1. ����  
�任����  
```  
|x'|   |MSCALE_X	0		0| |x|  
|y'| = |0		MSCALE_Y	0| |y|  
|1|    |0		0			1| |1|  
```  
�����  
x' = MSCALE_X*x + 0*y + 0*1 = MSCALE_X*x  
y' = 0*x + MSCALE_Y*y + 0*1 = MSCALE_Y*y  
 