# Paint详解  
## Paint API分类  
* 颜色  
* 效果  
* drawText()相关  
* 初始化  
## Paint颜色API  
### 基本颜色  
我们在使用canvas的时候，可以使用其drawColor等相关方法来直接填充颜色，而drawBitmap中的颜色是Bitmap对象提供的，其余的绘制，需要设置颜色时，就需要使用到Paint了  
### Paint设置基本颜色  
#### 直接设置颜色  
* setColor(int color);  
* setARGB(int a, int r, int g, int b);  
***  
#### 设置Shader  
> Shader:着色器，是一个颜色方案，设置了Shader时，Paint不再使用setColor/setARGB直接设置的颜色  
  
##### Android中Shader的子类  
> Android中不直接使用Shader，而是使用其的几个子类  
  
---  
###### LinearGradient:线性渐变  
*两种颜色：*  
> LinearGradient(float x0, float y0, float x1, float y1, int color0, int color1, TileMode tile)  
  
参数说明：  
* (x0, y0),(x1, y1):两个端点的坐标  
* color0, color1:两个端点的颜色，color0对应端点(x0,y0)的颜色，color1对应端点(x1,y1)的颜色  
  
*多种颜色：*  
> LinearGradient(float x0, float y0, float x1, float y1, int colors[], float positions[], TileMode tile)  
  
参数说明：  
* (x0, y0),(x1, y1):两个端点的坐标  
* int colors[]:颜色数组  
* float positions[]:颜色数组中各个颜色对应的位置，这里的每个值的取值范围是[0-1]，这里就理解成该位置与起始端点的距离占开始端点与结束端点距离（就是上面两个端点直接的距离）的比例就可以了  
---  
###### RadialGradient:辐射渐变  
*两种颜色：*  
> RadialGradient(float centerX, float centerY, float radius, int centerColor, int edgeColor, TileMode tileMode)  
  
参数说明：  
* (centerX, centerY), radius:原点以及辐射半径  
* centerColor, edgeColor:中心点颜色和边缘颜色(这个构造方法中，中心点颜色向边缘扩散，边缘颜色向中心扩散，所有表现出来两种颜色的混合渐变范围各占半径的一半)  
  
*多种颜色：*  
> RadialGradient(float centerX, float centerY, float radius, int colors[], float stops[], TileMode tileMode)  
  
参数说明：
* (centerX, centerY), radius:原点以及辐射半径  
* colors:颜色数组  
* stops:这里的官方因为解释确实没看懂，只知道取值范围是0-1，我实际测试了一下，实际的表现是该位置上的颜色（即：colors[index]）开始向中心和边缘混合渐变的离圆心的距离（即：radius*stops[index]）  
> 特别说明：stops[0]的值不是0的话，第一个颜色的开始渐变位置到圆心的范围内都着色为第一个颜色，不会渐变（这也好理解，第一个颜色之前没有颜色，自然它向中心扩散时就没有别的颜色来混合，自然也就没有渐变咯）  
---  
###### SweepGradient:扫描渐变  
*两种颜色：*  
> SweepGradient(float cx, float cy, int color0, int color1)  
  
参数说明：  
* (cx, cy):圆心坐标  
* color0, color1:color0表示沿时针方向0度的颜色，color1表示沿时针方向360度的颜色（所以这个着色规则是颜色从角度向更小角度和更大角度扩散混合）  
  
*多种颜色：*  
> SweepGradient(float cx, float cy, int colors[], float positions[])  
  
参数说明：  
* (cx, cy):圆心坐标  
* colors:颜色数组  
* positions:取值范围：0-1，表示相应的颜色更小角度和更大角度开始扩散混合的角度（即：360*positions[index]）  
---  
###### BitmapShader:图片着色  
> BitmapShader(Bitmap bitmap, TileMode tileX, TileMode tileY)  
> 说明：这里的图片是从当前坐标原点开始绘制（就是图片的左上角在当前坐标原点），其余两个参数为图片在横纵坐标上的，图片大小范围外的着色规则  
---  
###### ComposeShader:混合作色器  
> ComposeShader(Shader shaderA, Shader shaderB, Xfermode mode)  
> ComposeShader(Shader shaderA, Shader shaderB, PorterDuff.Mode mode)  
  
*TileMode:着色规则*  
> 是定义的范围外的颜色着色规则  
* Shader.TileMode.CLAMP:颜色从端点向周围发散  
```  
	LinearGradient linearGradient = new LinearGradient(-200, 0, 200, 0, Color.RED, Color.BLUE, Shader.TileMode.CLAMP);
        paint.setShader(linearGradient);
        canvas.drawRect(-width/2, -height/2, width/2, height/2, paint);  
```  
![](./images/tilemode_clamp.png)  
* Shader.TileMode.MIRROR:颜色从两端点向中间发散，然后沿着两端点连线方向镜像颜色  
```  
	LinearGradient linearGradient = new LinearGradient(-200, 0, 200, 0, Color.RED, Color.BLUE, Shader.TileMode.MIRROR);
        paint.setShader(linearGradient);
        canvas.drawRect(-width/2, -height/2, width/2, height/2, paint);  
```  
![](./images/tilemode_mirror.png)  
* Shader.TileMode.REPEAT:颜色从两端点向中间发散，然后沿着两端点连线方向重复颜色  
```  
	LinearGradient linearGradient = new LinearGradient(-200, 0, 200, 0, Color.RED, Color.BLUE, Shader.TileMode.REPEAT);
        paint.setShader(linearGradient);
        canvas.drawRect(-width/2, -height/2, width/2, height/2, paint);  
```  
![](./images/tilemode_repeat.png)  
*多颜色实例*  
```  
	canvas.drawColor(Color.BLACK);
        canvas.translate(width*1.0f/2, height*1.0f/2);
        String text = "鲁迅加油";
        paint.setTextSize(150);
        float offset = paint.measureText(text)/2;
        int[] color = {Color.parseColor("#2ad018"), Color.parseColor("#fe002e"), Color.parseColor("#d5207f"), Color.parseColor("#0b6dd8")};
        float[] position = {0f, 0.5f, 0.5f, 1f};
        LinearGradient linearGradient = new LinearGradient(-offset, 0, offset, 0, color, position, Shader.TileMode.CLAMP);
        paint.setShader(linearGradient);
        canvas.drawText(text, -offset, 0, paint);  
```  
![](./images/lineargradient_multi_color.png)  
