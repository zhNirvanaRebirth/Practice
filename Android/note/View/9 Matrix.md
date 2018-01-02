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
> ����ĵ㶼��ʹ���������ϵ����ʾ�ģ�(x, y, 1)��ʾ�㣻(x, y, 0)��ʾ����  
  
1. ����  
�任����  
```  
|x'|   |MSCALE_X	0		0| |x|  
|y'| = |0		MSCALE_Y	0| |y|  
|1|    |0		0		1| |1|  
```  
�����  
x' = MSCALE_X\*x + 0\*y + 0\*1 = MSCALE_X\*x  
y' = 0\*x + MSCALE_Y\*y + 0\*1 = MSCALE_Y\*y  
2. ����  
ˮƽ���У��任����  
```  
|x'|   |1	MSKEW_X	0| |x|  
|y'| = |0	1	0| |y|  
|1|    |0	0	1| |1|  
```  
�����  
x' = x + MSKEW_X\*y + 0\*1 = x + MSKEW_X\*y  
y' = 0\*x + 1\*y + 0\*1 = y  
��ֱ���У��任����  
```  
|x'|   |1	0	0| |x|  
|y'| = |MSKEW_Y	1	0| |y|  
|1|    |0	0	1| |1|  
```  
�����  
x' = x + 0\*y + 0\*1 = x  
y' = MSKEW_Y\*x + 1\*y + 0\*1 = MSKEW_Y\*x + y  
���ϴ��У��任����  
```  
|x'|   |1	MSKEW_X	0| |x|  
|y'| = |MSKEW_Y	1	0| |y|  
|1|    |0	0	1| |1|  
```  
�����  
x' = x + MSKEW_X\*y + 0\*1 = x + MSKEW_X\*y  
y' = MSKEW_Y\*x + 1\*y + 0\*1 = MSKEW_Y\*x + y  
3. ��ת  
��һ��(x, y),���������е�λ�ÿ��Ա�ʾΪ��  
```  
x = rcos��  
y = rsin��  
```  
(x, y)��ԭ����ת�Ȼ��ȣ���ת�õ��ĵ�����꣺  
```  
x' = rcos(�� + ��) = rcos��cos�� - rsin��sin�� = xcos�� - ysin��  
y' = rsin(�� + ��) = rsin��cos�� + rcos��sin�� = ycos�� + xsin��  
```  
�����ʾ��  
```  
|x'|   |cos��	-sin��	0| |x|  
|y'| = |sin��	cos��	0| |y|  
|1|    |0	0	1| |1|  
```  
4. ƽ��  
�任����  
```  
|x'|   |1	0	x1| |x|  
|y'| = |0	1	y1| |y|  
|1|    |0	0	 1| |1|  
```  
�����  
x' = 1\*x + 0\*y + x1\*1 = x + x1  
y' = 0\*x + 1\*y + y1\*1 = y + y1  
  