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
### MotionEvent_ACTION_POINTER_DOWN/MotionEvent.ACTION_POINTER_UP����ָ��ActionIndex�Ĺ�ϵ  
> ÿ����ָ�ڰ���ʱ�͵õ�һ��ActionIndex����ͨ��getActionIndex()������ã�����ָһֱ���µ�״̬�����Ӧ��ActionIndex����һֱ����ģ���������м������  
  
* ActionIndex��0��ʼ���ڶ������µ���ָ��Ӧ��ActionIndexΪ1����������  
* ��������MotionEvent.ACTION_POINTER_UP�¼�ʱ��̧����ָ���������̧�����ָ��ActionIndex�����գ�ʣ�µ�δ̧�����ָ��ActionIndex�������仯����ʱ����MotionEvent_ACTION_POINTER_DOWN�¼���������ָ�������µ���ָ��ActionIndex�������С��δʹ�õ����֣��������ڰ�����������ָ����ActionIndex����Ϊ0,2,4����ô���ڰ��������ָ��ActionIndex=1��  
* ������η���MotionEvent.ACTION_POINTER_UP�¼�ʱ��̧����ָ���������ֻ��̧�����ָ��ActionIndex�������ģ�ʣ�µ�δ̧�����ָ��ActionIndex�ᷢ���仯����ʱ����MotionEvent_ACTION_POINTER_UP�¼�������̧����ָ�����ڴ�֮ǰ���������Ϊ����̧����ָ֮ǰ����ʣ����ָ��ActionIndex�������仯���������ActionIndex�����Ǵ�0��ʼ�������ģ�����ԭ��������4����ָ��ActionIndex=0��1�� 2�� 3������̧����ActionIndex=2����ָ����ô��������̧����ĸ����µ���ָ��ԭ����ActionIndex=3����ʵ�������ǵõ����ڴ���̧���¼�����ָ��ActionIndex=2������ô����  

### ���㴥�����㴥�ص��¼���ȡ  
* ��㴥�ر���ʹ��getActionMasked()����ȡ�¼�����  
* ���㴥��ʹ��getAction()��getActionMasked()����ȡ�¼����Ͷ���  
* ʹ�÷���getActionIndex()����ȡ��������¼�����ָ��ActionIndex������getActionIndex()����ֻ��down��up��Ч���������ĸ���ָ��move�¼������ǻ�ȡ����ActionIndex����0���������Ч��  
### ��ʷ����  
> ��ʷ����ֻ��ACTION_MOVE�¼�  
  
* getHistorySize():��ȡ��ʷ�¼����ϴ�С  
* getHistoricalX(int pos):��ȡ��pos����ʷ�¼�X����  
* getHistoricalY(int pos):��ȡ��pos����ʷ�¼�Y����  
* getHistoricalX(int actionIndex, int pos):��ȡ��actionIndex��ָ�ĵ�pos����ʷ�¼���X����  
* getHistoricalY(int actionIndex, int pos):��ȡ��actionIndex��ָ�ĵ�pos����ʷ�¼���Y����  
### ��ȡ�¼�������ʱ��  
* getDownTime():��ȡ��ָ���µ�ʱ��  
* getEventTime():��ȡ��ǰ�¼�������ʱ��  
* getHistoricalEventTime(int pos):��ȡ��pos����ʷ�¼�������ʱ��  
### ��ȡ�Ӵ����/ѹ��  
> ��ȡ�Ӵ������С��ѹ����С��ҪӲ��֧�֣��е��豸ֻ֧������һ�����еĿ��ܶ���֧��  
  
* getSize():��ȡ��һ����ָ����Ļ�Ӵ�����Ĵ�С  
* getSize(int actionIndex):��ȡ��actionIndex����ָ����Ļ�Ӵ�����Ĵ�С  
* getHistoricalSize(int pos):��ȡ��ʷ�����е�һ����ָ�ڵ�pos���¼��еĽӴ������С  
* getHistoricalSize(int actionIndex, int pos):��ȡ��ʷ�����е�actionIndex����ָ�ڵ�pos���¼��еĽӴ������С  
* getPressure():��ȡ��һ����ָ��ѹ����С  
* getPressure(int actionIndex):��ȡ��actionIndex����ָ��ѹ����С  
* getHistoricalPressure(int pos):��ȡ��ʷ�����е�һ����ָ�ڵ�pos�¼��е�ѹ����С  
* getHistoricalPressure(int actionIndex, int pos):��ȡ��ʷ�����е�actionIndex����ָ�ڵ�pos�¼��е�ѹ����С  
