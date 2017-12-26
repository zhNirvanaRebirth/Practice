# Canvas��������  
**Canvas�Ĳ���ֻ�Ժ������������ã����ѻ��Ƶ�����û��Ӱ��**  
## λ��(translate)  
> λ����Ե��ǵ�ǰ���꣬Ҳ����˵canvas��λ���ǵ��ӵ�  
  
## ����(scale)  
> ������scale(float sx, float sy)/scale(float sx, float sy, float px, float py)  
  
����˵����  
* sx,sy����ʾx�����y�����ϵ����ű���  
* (px,py)����ʾ�������ĵ����꣬��û��ָ��������������ʱ��Ĭ�ϵ���������Ϊ��ǰ������ԭ�㣨�����ű���Ϊ��ֵʱ���������ݻ��������������Ϣ��ת��  
### ���ű���������������ս����ϵ��  
* ���ű�����С��[-��,-1)    ���ƽ�������������ȸ����������ķŴ�n���������������ķ�ת  
* ���ű�����С��-1    ���ƽ���������������������ķ�ת  
* ���ű�����С��(-1,0)    ���ƽ�������������ȸ�������������Сn���������������ķ�ת  
* ���ű�����С��0    ���ƽ�����������κ����ݣ���Ϊ��СΪ0��  
* ���ű�����С��(0, 1)    ���ƽ�����������ݸ�������������Сn��  
* ���ű�����С��1    ���ƽ�����������ݲ������仯  
* ���ű�����С��(1, +��)    ���ƽ�����������ݸ����������ķŴ�n��  
**scaleҲ�ǿ��Ե��ӵ�**  
## ��ת(rotate)  
> ������rotate(float degrees)/rotate(float degrees, float px, float py)  
  
����˵����  
* degrees:��ת�Ƕȣ�Ĭ������£���ת�����ǵ�ǰ������ԭ��  
* (px,py):��ת��������  
**��תҲ�ǿɵ��ӵ�**  
## ����(skew)  
> ������skew(float sx, float sy)  
  
����˵����  
* sx������������x������һ����б�������б�ǵ�tanֵΪsx��sx=��������ĳ����x���ϵ���б����/�õ�y�����곤�ȣ�  
* sy������������y������һ����б�������б�ǵ�tanֵΪsy��sy=��������ĳ����y���ϵ���б����/�õ�x�����곤�ȣ�  
```  
һ�������(x,y)��������sx,sy֮��õ�һ������(X,Y)����X��Y��ֵΪ��  
X=x + sx*y;  
Y=y + sy*x;  
```  
## ����(save)��ع�(restore)  
�����Ĳ����ǲ�����ģ���˵�������Ҫʹ������canvas��������һ�����ݻ��ƣ�Ȼ�����������ǰ��������������Ļ��ƣ���ʱ����Ҫ�õ�canvas�Ŀ��պͻع�  
������ʽ��  
```  
canvas.save();
...  
canvas.restore();  
```  
## ͼƬ����  
### ʸ��ͼ����  
> ����������ʹ��Picture¼���������ٵ�����ط�������ʵ�ʻ���  
  
Picture����ط�����  
* Canvas beginRecording(int width, int height)����ʼ¼�ƣ�����ָ��ʸ��ͼ�ĳߴ磬������һ��Canvas�������ǿ��������Canvas�����������ǵĻ��ƣ�ƽʱ��onDraw����ôʹ��canvas�ģ�������ôʹ�þ����ˣ�  
* void endRecording()�����¼��  
��Picture���ݻ��Ƴ����ķ�����  
* Picture��draw��������Canvas��״̬����Ӱ�죬һ�㲻ʹ��  
* Canvas��drawPicture����  
* ��Picture��װ��PictureDrawable��ʹ��PictureDrawable��draw����  
**Canvas�ڻ���Picture��ʱ��ͻ���Bitmap��һ���ģ��ӵ�ǰ����ԭ�㿪ʼ����Picture�����Ͻǣ�Picture�Ĵ�С��beginRecording��ʱ��ָ��**  
Canvas��Picture�Ļ���  
* drawPicture(@NonNull Picture picture)������Picture��¼�Ƶ�����  
* drawPicture(@NonNull Picture picture, @NonNull Rect dst)/drawPicture(@NonNull Picture picture, @NonNull RectF dst):��Picture������ָ�������������Picture�Ĵ�С��һ�£�Picture�������Ӧ������  
��Picture��װ��PictureDrawable  
> ��Ҫע��һ�����⣬������Ҫʹ��PictureDrawable��setBounds������Picture�Ļ�������ע�⣬���ﲻ�����ţ�Ҳ����ü�Picture����ֻ������PictureҪ���Ƶ�����  
  
### λͼ����  
Bitmap��ȡ��ʽ��  
* ͨ��Bitmap����  
* ͨ��BitmapDrawable��ȡ  
* ͨ��BitmapFactory��ȡ  
Canvas��Bitmap�Ļ���  
* drawBitmap(@NonNull Bitmap bitmap, @NonNull Matrix matrix, @Nullable Paint paint)����canvas���м��α任������λͼ  
* drawBitmap(@NonNull Bitmap bitmap, @Nullable Rect src, @NonNull RectF dst, @Nullable Paint paint)/drawBitmap(@NonNull Bitmap bitmap, @Nullable Rect src, @NonNull Rect dst, @Nullable Paint paint):��ͼƬ�е�src�������ݻ��Ƶ�canvas��dst�����������С����ʱ��������  
* drawBitmap(@NonNull Bitmap bitmap, float left, float top, @Nullable Paint paint)���ӵ�ǰcanvas������ϵ�еĵ�(left,top)��ʼ����Bitmap  
  
