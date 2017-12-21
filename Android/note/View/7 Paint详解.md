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
***  
#### ����Shader  
> Shader:��ɫ������һ����ɫ������������Shaderʱ��Paint����ʹ��setColor/setARGBֱ�����õ���ɫ  
  
##### Android��Shader������  
> Android�в�ֱ��ʹ��Shader������ʹ����ļ�������  
  
---  
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
---  
###### RadialGradient:���佥��  
*������ɫ��*  
> RadialGradient(float centerX, float centerY, float radius, int centerColor, int edgeColor, TileMode tileMode)  
  
����˵����  
* (centerX, centerY), radius:ԭ���Լ�����뾶  
* centerColor, edgeColor:���ĵ���ɫ�ͱ�Ե��ɫ(������췽���У����ĵ���ɫ���Ե��ɢ����Ե��ɫ��������ɢ�����б��ֳ���������ɫ�Ļ�Ͻ��䷶Χ��ռ�뾶��һ��)  
  
*������ɫ��*  
> RadialGradient(float centerX, float centerY, float radius, int colors[], float stops[], TileMode tileMode)  
  
����˵����
* (centerX, centerY), radius:ԭ���Լ�����뾶  
* colors:��ɫ����  
* stops:����Ĺٷ���Ϊ����ȷʵû������ֻ֪��ȡֵ��Χ��0-1����ʵ�ʲ�����һ�£�ʵ�ʵı����Ǹ�λ���ϵ���ɫ������colors[index]����ʼ�����ĺͱ�Ե��Ͻ������Բ�ĵľ��루����radius*stops[index]��  
> �ر�˵����stops[0]��ֵ����0�Ļ�����һ����ɫ�Ŀ�ʼ����λ�õ�Բ�ĵķ�Χ�ڶ���ɫΪ��һ����ɫ�����ὥ�䣨��Ҳ����⣬��һ����ɫ֮ǰû����ɫ����Ȼ����������ɢʱ��û�б����ɫ����ϣ���ȻҲ��û�н��俩��  
---  
###### SweepGradient:ɨ�轥��  
*������ɫ��*  
> SweepGradient(float cx, float cy, int color0, int color1)  
  
����˵����  
* (cx, cy):Բ������  
* color0, color1:color0��ʾ��ʱ�뷽��0�ȵ���ɫ��color1��ʾ��ʱ�뷽��360�ȵ���ɫ�����������ɫ��������ɫ�ӽǶ����С�ǶȺ͸���Ƕ���ɢ��ϣ�  
  
*������ɫ��*  
> SweepGradient(float cx, float cy, int colors[], float positions[])  
  
����˵����  
* (cx, cy):Բ������  
* colors:��ɫ����  
* positions:ȡֵ��Χ��0-1����ʾ��Ӧ����ɫ��С�ǶȺ͸���Ƕȿ�ʼ��ɢ��ϵĽǶȣ�����360*positions[index]��  
---  
###### BitmapShader:ͼƬ��ɫ  
> BitmapShader(Bitmap bitmap, TileMode tileX, TileMode tileY)  
> ˵���������ͼƬ�Ǵӵ�ǰ����ԭ�㿪ʼ���ƣ�����ͼƬ�����Ͻ��ڵ�ǰ����ԭ�㣩��������������ΪͼƬ�ں��������ϵģ�ͼƬ��С��Χ�����ɫ����  
---  
###### ComposeShader:�����ɫ��  
> ComposeShader(Shader shaderA, Shader shaderB, Xfermode mode)  
> ComposeShader(Shader shaderA, Shader shaderB, PorterDuff.Mode mode)  
  
---  
*TileMode:��ɫ����*  
> �Ƕ���ķ�Χ�����ɫ��ɫ����  
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
---  
*PorterDuff.Mode:��ɫ����*  
> ����ָ������ͼ��/ͼ��ͬ����ʱ����ɫ���ԣ�Դͼ�λ��Ƶ�Ŀ��ͼ�δ�ʱӦ����ôȷ�����߽�Ϻ����ɫ  
  
**�Ƚ���Ҫ��һ�㣺����DST������Ķ��ǿ���Դͼ����������ʲô�Ϳ����ˣ�����������ѻ�����ʲô���Ǿ���ʲô**  
* SRC��ֻ����Դͼ�Σ�ע�⣬�����ֻ����Դͼ��/ͼ����ָ�����򣬽�������Դͼ��/ͼ���С�ĵط�����������Դͼ�κ�Ŀ��ͼ�δ�С����һ��Ƶ������غϣ���Ȼ��ֻ����Դͼ�Σ�pngͼƬ��͸��������ͼƬ�Ĵ�С��������ɫ��ϣ�������Ҫ��Ŀ��ͼ�δ�С��Դͼ�δ󣬳���Դͼ�δ�С�Ļ���������Ȼ���ǻ���Ŀ��ͼ�Σ���������ɫ���� **����Ҫע�⣬������ʹ��canvas����ͼ�Σ���ô����ͼ�ε�����ʹ�С�����Ѷ��ģ����ϻ�������Ҫ��⣡**  
* DST��ֻ����Ŀ��ͼ�Σ��������ɣ�����Դͼ����������ֱ�ӾͲ�������  
* DST_IN������Ŀ��ͼ����Դͼ���еĲ��֣�**��������ע�⣬�����ϵ���������Դͼ������Ϊ׼������Դͼ�δ�С������Ȼ�û���ʲô�ͻ���ʲô**  
* CLEAR����Դͼ����������ͼ�Σ�������Դͼ�λ���Ŀ��ͼ�ζ���գ������л��ƣ�  
���Դ��룺  
```  
	int saveCount = canvas.saveLayer(null, null, Canvas.ALL_SAVE_FLAG);
        paint.setColor(Color.BLUE);
        canvas.drawRect(-200, -200, 200, 200, paint);
        PorterDuffXfermode xfermode = new PorterDuffXfermode(PorterDuff.Mode.DST_IN);
        paint.setXfermode(xfermode);
        paint.setColor(Color.RED);
        canvas.drawCircle(200, 200, 200, paint);
        paint.setXfermode(null);
        canvas.restoreToCount(saveCount);  
```  
### setColorFilter(ColorFilter colorFilter)  
> �Ի��Ƶ����ݵ�ÿ�����ؽ��й��˺��ڻ��Ƴ��������õ�ColorFilterΪ������  
  
#### LightingColorFilter  
> ���췽����LightingColorFilter(int mul, int add);  
  
����˵����  
* mul������ɫֵ��ʽ��ͬ��intֵ���磺0xffffff����������Ŀ���������  
* add������ɫֵ��ʽ��ͬ��intֵ���磺0xffffff����������Ŀ���������  
����ԭ�����£�  
R' = R * mul.R / 0xff + add.R  
G' = G * mul.G / 0xff + add.G  
B' = B * mul.B / 0xff + add.B  
**������ʲô���أ���������ɾ��Ŀ���е�ĳ��/ĳЩ��ɫ���ߵ���Ŀ����ĳ��/ĳЩ��ɫ����ǳ**  
#### PorterDuffColorFilter  
> ���췽����PorterDuffColorFilter(@ColorInt int color, @NonNull PorterDuff.Mode mode);  
  
����˵����  
* color��������Ŀ���ϵ���ɫ  
* mode����ɫ���ģʽ  
**�����ٴ�ʹ�õ���PorterDuff.Mode,ֻ��������ʹ�õ���ɫ��ΪԴ������������ܻ��õ���mode���ǣ�DARKEN��LIGHTEN��MULTIPLY��**  
## Ч��  
> Paint����ʹ��һЩapi��ʹ�������ݱ��ֳ���ͬ��Ч��  
  
### Paint����������״  
* setStrokeWidth(float width)������������ȣ�Ĭ�������Ŀ����0�����ǻ��������������Ȼ��1�����أ����ֵ�ĺô��ǵ����ǶԻ���ͼ����б任����������ʱ����߿�������Ȳ��ᷢ�����ţ�  
* setStrokeCap(Paint.Cap cap)��������ͷ��״��BUTT ƽͷ��ROUND Բͷ��SQUARE ��ͷ��Ĭ����BUTT��  
* setStrokeJoin(Paint.Join join)���������߹սǵ���״��MITER ��ǣ�BEVEL ƽ�ǣ�ROUND Բ�ǣ�Ĭ����MITER��  
* setStrokeMiter(float miter)����setStrokeJoin������һ�����䣬��joinΪMITERʱ�����ƹս��ӳ��ߵ����ֵ���������ߵļнǹ�С��ʱ�򣬻���ɼ�ǹ�����������⣩  
���Դ��룺  
```  
	paint.setStrokeWidth(50);
        paint.setColor(Color.BLACK);
        paint.setStyle(Paint.Style.STROKE);
        paint.setStrokeJoin(Paint.Join.ROUND);
        Path path= new Path();
        path.moveTo(-200, -200);
        path.lineTo(200, -200);
        path.lineTo(0, 200);
        path.close();
        canvas.drawPath(path, paint);  
```  
## ɫ���Ż�  
* setDither(boolean dither)���Ƿ���붶��������������������ͼ�񽵵�ɫ����Ȼ���ʱ��������ִ�Ƭ��ɫ����ɫ�飨����һƬ�������ɫ�����ŵ�һƬ������������ɫ����ɫ���ȹ���  
* setFilterBitmap(boolean filter)���Ƿ�ʹ��˫���Թ���������Bitmap������˫���Թ��ˣ����Ա���ͼ���ڷŴ���Ƶ�ʱ���������������  
