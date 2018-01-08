# Canvas画布操作  
**Canvas的操作只对后续操作起作用，对已绘制的内容没有影响**  
## 位移(translate)  
> 位移相对的是当前坐标，也就是说canvas的位移是叠加的  
  
## 缩放(scale)  
> 方法：scale(float sx, float sy)/scale(float sx, float sy, float px, float py)  
  
参数说明：  
* sx,sy：表示x方向和y方向上的缩放比例  
* (px,py)：表示缩放中心的坐标，当没有指定缩放中心坐标时，默认的缩放中心为当前的坐标原点（当缩放比例为负值时，绘制内容会根据缩放中心信息翻转）  
### 缩放比例与绘制内容最终结果关系：  
* 缩放比例大小：[-∞,-1)    绘制结果：绘制内容先根据缩放中心放大n倍，再沿缩放中心翻转  
* 缩放比例大小：-1    绘制结果：绘制内容沿缩放中心翻转  
* 缩放比例大小：(-1,0)    绘制结果：绘制内容先根据缩放中心缩小n倍，再沿缩放中心翻转  
* 缩放比例大小：0    绘制结果：不绘制任何内容（因为大小为0）  
* 缩放比例大小：(0, 1)    绘制结果：绘制内容根据缩放中心缩小n倍  
* 缩放比例大小：1    绘制结果：绘制内容不发生变化  
* 缩放比例大小：(1, +∞)    绘制结果：绘制内容根据缩放中心放大n倍  
**scale也是可以叠加的**  
## 旋转(rotate)  
> 方法：rotate(float degrees)/rotate(float degrees, float px, float py)  
  
参数说明：  
* degrees:旋转角度，默认情况下，旋转中心是当前的坐标原点  
* (px,py):旋转中心坐标  
**旋转也是可叠加的**  
## 错切(skew)  
> 方法：skew(float sx, float sy)  
  
参数说明：  
* sx：绘制内容在x方向做一个倾斜，这个倾斜角的tan值为sx（sx=绘制内容某点在x轴上的倾斜距离/该点y轴坐标长度）  
* sy：绘制内容在y方向做一个倾斜，这个倾斜角的tan值为sy（sy=绘制内容某点在y轴上的倾斜距离/该点x轴坐标长度）  
```  
一个坐标点(x,y)经过错切sx,sy之后得到一个坐标(X,Y)，则X，Y的值为：  
X=x + sx*y;  
Y=y + sy*x;  
```  
## 快照(save)与回滚(restore)  
画布的操作是不可逆的，因此当我们需要使用上述canvas操作进行一个内容绘制，然后继续按操作前的情况进行其他的绘制，这时就需要用到canvas的快照和回滚  
基本格式：  
```  
canvas.save();
...  
canvas.restore();  
```  
## 图片绘制  
### 矢量图绘制  
> 将绘制内容使用Picture录制下来，再调用相关方法进行实际绘制  
  
Picture的相关方法：  
* Canvas beginRecording(int width, int height)：开始录制，这里指令矢量图的尺寸，返回是一个Canvas对象，我们可以拿这个Canvas对象来做我们的绘制（平时在onDraw中怎么使用canvas的，这里怎么使用就是了）  
* void endRecording()：完成录制  
将Picture内容绘制出来的方法：  
* Picture的draw方法：对Canvas的状态会有影响，一般不使用  
* Canvas的drawPicture方法  
* 将Picture封装成PictureDrawable，使用PictureDrawable的draw方法  
**Canvas在绘制Picture的时候和绘制Bitmap是一样的，从当前坐标原点开始绘制Picture的左上角，Picture的大小在beginRecording的时候指定**  
  
Canvas对Picture的绘制  
* drawPicture(@NonNull Picture picture)：绘制Picture中录制的内容  
* drawPicture(@NonNull Picture picture, @NonNull Rect dst)/drawPicture(@NonNull Picture picture, @NonNull RectF dst):将Picture绘制在指定区域，若区域和Picture的大小不一致，Picture会进行相应的缩放  
将Picture包装成PictureDrawable  
> 需要注意一个问题，我们需要使用PictureDrawable的setBounds来设置Picture的绘制区域，注意，这里不会缩放，也不会裁剪Picture，而只是设置Picture要绘制的区域  
  
### 位图绘制  
Bitmap获取方式：  
* 通过Bitmap创建  
* 通过BitmapDrawable获取  
* 通过BitmapFactory获取  
  
Canvas对Bitmap的绘制  
* drawBitmap(@NonNull Bitmap bitmap, @NonNull Matrix matrix, @Nullable Paint paint)：对canvas进行几何变换并绘制位图  
* drawBitmap(@NonNull Bitmap bitmap, @Nullable Rect src, @NonNull RectF dst, @Nullable Paint paint)/drawBitmap(@NonNull Bitmap bitmap, @Nullable Rect src, @NonNull Rect dst, @Nullable Paint paint):将图片中的src区域内容绘制到canvas的dst区域，两区域大小不等时进行缩放  
* drawBitmap(@NonNull Bitmap bitmap, float left, float top, @Nullable Paint paint)：从当前canvas的坐标系中的点(left,top)开始绘制Bitmap  
  
## 画布裁剪  
### 范围裁剪  
* clipRect(int left, int top, int right, int bottom)：截取画布上rect部分的内容（就是只绘制rect区域的内容）  
* clipPath(@NonNull Path path)：截取画布上path部分的内容（就是只绘制path区域的内容）  
