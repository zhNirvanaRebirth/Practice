# Paint���  
## Paint API����  
* ��ɫ  
* Ч��  
* drawText()���  
* ��ʼ��  
## Paint��ɫAPI  
### ������ɫ  
������ʹ��canvas��ʱ�򣬿���ʹ����drawColor����ط�����ֱ�������ɫ����drawBitmap�е���ɫ��Bitmap�����ṩ�ģ�����Ļ��ƣ���Ҫ������ɫʱ������Ҫʹ�õ�Paint��  
### Paint���û�����ɫ  
#### ֱ��������ɫ  
* setColor(int color);  
* setARGB(int a, int r, int g, int b);  
#### ����Shader  
> Shader:��ɫ������һ����ɫ������������Shaderʱ��Paint����ʹ��setColor/setARGBֱ�����õ���ɫ  
  
##### Android��Shader������  
> Android�в�ֱ��ʹ��Shader������ʹ����ļ�������  
  
###### LinearGradient:���Խ���  
*������ɫ��*  
> LinearGradient(float x0, float y0, float x1, float y1, int color0, int color1, TileMode tile)  
  
����˵����  
* (x0, y0),(x1, y1):�����˵������  
* color0, color1:�����˵����ɫ��color0��Ӧ�˵�(x0,y0)����ɫ��color1��Ӧ�˵�(x1,y1)����ɫ  
  
*������ɫ��*  
> LinearGradient(float x0, float y0, float x1, float y1, int colors[], float positions[], TileMode tile)  
  
����˵����  
* (x0, y0),(x1, y1):�����˵������  
* int colors[]:��ɫ����  
* float positions[]:��ɫ�����и�����ɫ��Ӧ��λ�ã������ÿ��ֵ��ȡֵ��Χ��[0-1]����������ɸ�λ������ʼ�˵�ľ���ռ��ʼ�˵�������˵���루�������������˵�ֱ�ӵľ��룩�ı����Ϳ�����  

*TileMode:��ɫ����*  
* Shader.TileMode.CLAMP:��ɫ�Ӷ˵�����Χ��ɢ  
```  
	LinearGradient linearGradient = new LinearGradient(-200, 0, 200, 0, Color.RED, Color.BLUE, Shader.TileMode.CLAMP);
        paint.setShader(linearGradient);
        canvas.drawRect(-width/2, -height/2, width/2, height/2, paint);  
```  
![](./images/tilemode_clamp.png)  
* Shader.TileMode.MIRROR:��ɫ�����˵����м䷢ɢ��Ȼ���������˵����߷�������ɫ  
```  
	LinearGradient linearGradient = new LinearGradient(-200, 0, 200, 0, Color.RED, Color.BLUE, Shader.TileMode.MIRROR);
        paint.setShader(linearGradient);
        canvas.drawRect(-width/2, -height/2, width/2, height/2, paint);  
```  
![](./images/tilemode_mirror.png)  
* Shader.TileMode.REPEAT:��ɫ�����˵����м䷢ɢ��Ȼ���������˵����߷����ظ���ɫ  
```  
	LinearGradient linearGradient = new LinearGradient(-200, 0, 200, 0, Color.RED, Color.BLUE, Shader.TileMode.REPEAT);
        paint.setShader(linearGradient);
        canvas.drawRect(-width/2, -height/2, width/2, height/2, paint);  
```  
![](./images/tilemode_repeat.png)  
*����ɫʵ��*  
```  
	canvas.drawColor(Color.BLACK);
        canvas.translate(width*1.0f/2, height*1.0f/2);
        String text = "³Ѹ����";
        paint.setTextSize(150);
        float offset = paint.measureText(text)/2;
        int[] color = {Color.parseColor("#2ad018"), Color.parseColor("#fe002e"), Color.parseColor("#d5207f"), Color.parseColor("#0b6dd8")};
        float[] position = {0f, 0.5f, 0.5f, 1f};
        LinearGradient linearGradient = new LinearGradient(-offset, 0, offset, 0, color, position, Shader.TileMode.CLAMP);
        paint.setShader(linearGradient);
        canvas.drawText(text, -offset, 0, paint);  
```  
![](./images/lineargradient_multi_color.png)  
