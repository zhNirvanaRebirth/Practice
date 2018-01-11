# MotionEvent���  
## ���㴥��  
### ���㴥�ش������¼�  
* MotionEvent.ACTION_DOWN:��ָ���νӴ�����Ļʱ����  
* MotionEvent.ACTION_MOVE:��ָ����Ļ�ϻ���ʱ���������δ���  
* MotionEvent.ACTION_UP:��ָ�뿪��Ļʱ����  
* MotionEvent.ACTION_CANCEL:�¼����ϲ�����ʱ����  
* MotionEvent.ACTION_OUTSIDE:��ָ���ڿؼ����򴥷�  
### ���㴥�ص���ط���  
* getAction():��ȡ�¼�����  
* getX():��ȡ�������ڵ�ǰView��X�����꣨ԭ��ΪView�����Ͻǣ�  
* getY():��ȡ�������ڵ�ǰView��Y�����꣨ԭ��ΪView�����Ͻǣ�  
* getRawX():��ȡ��������������Ļ��X�����꣨ԭ��Ϊ��Ļ�����Ͻǣ�  
* getRawY():��ȡ��������������Ļ��Y�����꣨ԭ��Ϊ��Ļ�����Ͻǣ�  
### ���㴥���¼���MotionEvent.ACTION_CANCEL  
> MotionEvent.ACTION_CANCEL�ɳ��򴥷�,�����������¼����ϲ����أ�����Ҫ���¼����ϲ������ˣ�childView��ô�����յ��¼��أ����Ƕ�֪���¼��ַ������е�һ�������������¼�����һ��View���ѣ����һ��View�����˵�һ��ACTION_DOWN����ô�������¼�����ַ���Viewλ�ã����ǣ������и������һ��View������һ��ACTION_DOWN�����ǽ�������һ��ACTION_MOVE����������¼��ڷַ������View���ϲ�ʱ���ϲ��Viewȴ��Ҫ�������ACTION_MOVE�¼�������ϲ�View�����ACTION_MOVE�¼����أ���������£�View�ͻ��յ�һ��ACTION_CANCEL�¼����ҾͲ������յ��˴�ACTION_DOWN֮����¼���  
  
### ���㴥���¼���MotionEvent.ACTION_OUTSIDE  
> MotionEvent.ACTION_OUTSIDE��ָһ�������¼�������UIԪ�ص�������Χ֮�⣬�����¼������ڿؼ��ϣ���ô�����¼��ַ����ÿؼ����أ���ʵ����֮һ����Dialog�ĵ������ͼ������ر�Dialog����Ҫ������ͼ��Window��flagsΪWindowManager.LayoutParams.FLAG_NOT_TOUCH_MODAL���������£�û�гɹ��������ٿ���Dialog��Դ��ɣ�  
  
## ��㴥��  
> ��㴥���漰�������ָͬʱ������Ļ�ϣ�Ϊ�����ֶ����ָ�����Ĳ�ͬ�¼���Android��ʹ�ø�ÿ����ָ���,�����ж��¼������ĸ���ָ������  
  
### ��㴥�ص��¼�  
* MotionEvent.ACTION_DOWN:��һ����ָ���νӴ�����Ļʱ����  
* MotionEvent.ACTION_MOVE:��ָ����Ļ�ϻ���ʱ���������δ���  
* MotionEvent.ACTION_UP:���һ����ָ�뿪��Ļʱ����  
* MotionEvent_ACTION_POINTER_DOWN:����Ҫ��ָ����ʱ����(���ǵ�һ����ָ����)  
* MotionEvent.ACTION_POINTER_UP:����Ҫ��ָ̧��ʱ����(������ָ������Ļ��)  
### ��㴥�ص���ط���  
* getActionMasked():��getAction()���ͣ���㴥�ر���ʹ�����������ȡ�¼�����  
* getActionIndex():��ȡ�¼����ĸ���ָ������  
* getPointerCount():��ȡ��Ļ����ָ�ĸ���  
* getPointerId(int pointerIndex):��ȡһ����ָ��Ψһ��ʶ����һ����ָ�ڰ��º�̧��֮��ID���ֲ���  
* findPointerIndex(int pointerId):ͨ��pointerId�������Ӧ��pointerIndex����ָ��ţ�  
* getX(int pointerIndex):��ȡһ����ָ��X����  
* getY(int pointerIndex):��ȡһ����ָ��Y����  
