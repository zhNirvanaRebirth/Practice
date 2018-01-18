# MotionEvent详解  
## 单点触控  
### 单点触控触发的事件  
* MotionEvent.ACTION_DOWN:手指初次接触到屏幕时触发  
* MotionEvent.ACTION_MOVE:手指在屏幕上滑动时触发，会多次触发  
* MotionEvent.ACTION_UP:手指离开屏幕时触发  
* MotionEvent.ACTION_CANCEL:事件被上层拦截时触发  
* MotionEvent.ACTION_OUTSIDE:手指不在控件区域触发  
### 单点触控的相关方法  
* getAction():获取事件类型  
* getX():获取触摸点在当前View的X轴坐标（原点为View的左上角）  
* getY():获取触摸点在当前View的Y轴坐标（原点为View的左上角）  
* getRawX():获取触摸点在整个屏幕的X轴坐标（原点为屏幕的左上角）  
* getRawY():获取触摸点在整个屏幕的Y轴坐标（原点为屏幕的左上角）  
### 单点触控事件：MotionEvent.ACTION_CANCEL  
> MotionEvent.ACTION_CANCEL由程序触发,触发条件是事件被上层拦截，但是要是事件被上层拦截了，childView怎么还能收到事件呢？我们都知道事件分发机制中的一个规则是所有事件都被一个View消费，如果一个View消费了第一个ACTION_DOWN，那么后续的事件都会分发到View位置，但是，现在有个情况是一个View处理了一个ACTION_DOWN，但是接着来了一个ACTION_MOVE，但是这个事件在分发到这个View的上层时，上层的View却需要处理这个ACTION_MOVE事件，因此上层View将这个ACTION_MOVE事件拦截，这种情况下，View就会收到一个ACTION_CANCEL事件，且就不会再收到此次ACTION_DOWN之后的事件了  
  
### 单点触控事件：MotionEvent.ACTION_OUTSIDE  
> MotionEvent.ACTION_OUTSIDE是指一个触摸事件发生在UI元素的正常范围之外，但是事件都不在控件上，怎么会有事件分发到该控件上呢？其实例子之一就是Dialog的点击其视图区域外关闭Dialog，需要设置视图的Window的flags为WindowManager.LayoutParams.FLAG_NOT_TOUCH_MODAL（我试了下，没有成功，后面再看下Dialog的源码吧）  
  
## 多点触控  
> 多点触控涉及到多个手指同时按在屏幕上，为了区分多个手指产生的不同事件，Android中使用给每个手指编号,方便判断事件是由哪个手指触发的  
  
### 多点触控的事件  
* MotionEvent.ACTION_DOWN:第一个手指初次接触到屏幕时触发  
* MotionEvent.ACTION_MOVE:手指在屏幕上滑动时触发，会多次触发  
* MotionEvent.ACTION_UP:最后一个手指离开屏幕时触发  
* MotionEvent_ACTION_POINTER_DOWN:非主要手指按下时触发(不是第一个手指按下)  
* MotionEvent.ACTION_POINTER_UP:非主要手指抬起时触发(还有手指按在屏幕上)  
### 多点触控的相关方法  
* getActionMasked():与getAction()类型，多点触控必须使用这个方法获取事件类型  
* getActionIndex():获取事件是哪个手指产生的  
* getPointerCount():获取屏幕上手指的个数  
* getPointerId(int pointerIndex):获取一个手指的唯一标识符，一个手指在按下和抬起之间ID保持不变  
* findPointerIndex(int pointerId):通过pointerId查找其对应的pointerIndex（手指编号）  
* getX(int pointerIndex):获取一个手指的X坐标  
* getY(int pointerIndex):获取一个手指的Y坐标  
### MotionEvent_ACTION_POINTER_DOWN/MotionEvent.ACTION_POINTER_UP与手指的ActionIndex的关系  
> 每个手指在按下时就得到一个ActionIndex，可通过getActionIndex()方法获得，在手指一直按下的状态，其对应的ActionIndex不是一直不变的，具体可以有几种情况  
  
* ActionIndex以0开始，第二个按下的手指对应的ActionIndex为1，依次类推  
* 发生单次MotionEvent.ACTION_POINTER_UP事件时（抬起手指）的情况：抬起的手指的ActionIndex被回收，剩下的未抬起的手指的ActionIndex不发生变化，此时发生MotionEvent_ACTION_POINTER_DOWN事件（按下手指），按下的手指的ActionIndex会等于最小的未使用的数字（比如现在按下了三个手指，其ActionIndex依次为0,2,4，那么现在按下这个手指的ActionIndex=1）  
* 连续多次发生MotionEvent.ACTION_POINTER_UP事件时（抬起手指）的情况：只有抬起的手指的ActionIndex不是最大的，剩下的未抬起的手指的ActionIndex会发生变化，此时发生MotionEvent_ACTION_POINTER_UP事件（继续抬起手指），在此之前（可以理解为继续抬起手指之前），剩下手指的ActionIndex将发生变化，具体就是ActionIndex必须是从0开始且连续的（比如原来按下了4个手指，ActionIndex=0，1， 2， 3，现在抬起了ActionIndex=2的手指，那么我现在再抬起第四个按下的手指（原来的ActionIndex=3），实际上我们得到现在触发抬起事件的手指的ActionIndex=2，懂了么？）  

### 单点触控与多点触控的事件获取  
* 多点触控必须使用getActionMasked()来获取事件类型  
* 单点触控使用getAction()或getActionMasked()来获取事件类型都行  
* 使用方法getActionIndex()来获取触发相关事件的手指的ActionIndex，但是getActionIndex()方法只对down和up有效，而无论哪个手指的move事件，我们获取到的ActionIndex都是0，因此是无效的  
### 历史数据  
> 历史数据只有ACTION_MOVE事件  
  
* getHistorySize():获取历史事件集合大小  
* getHistoricalX(int pos):获取第pos个历史事件X坐标  
* getHistoricalY(int pos):获取第pos个历史事件Y坐标  
* getHistoricalX(int actionIndex, int pos):获取第actionIndex手指的第pos个历史事件的X坐标  
* getHistoricalY(int actionIndex, int pos):获取第actionIndex手指的第pos个历史事件的Y坐标  
### 获取事件发生的时间  
* getDownTime():获取手指按下的时间  
* getEventTime():获取当前事件发生的时间  
* getHistoricalEventTime(int pos):获取第pos个历史事件发生的时间  
### 获取接触面积/压力  
> 获取接触面积大小和压力大小需要硬件支持，有的设备只支持其中一个，有的可能都不支持  
  
* getSize():获取第一个手指与屏幕接触面积的大小  
* getSize(int actionIndex):获取第actionIndex个手指与屏幕接触面积的大小  
* getHistoricalSize(int pos):获取历史数据中第一个手指在第pos次事件中的接触面积大小  
* getHistoricalSize(int actionIndex, int pos):获取历史数据中第actionIndex个手指在第pos次事件中的接触面积大小  
* getPressure():获取第一个手指的压力大小  
* getPressure(int actionIndex):获取第actionIndex个手指的压力大小  
* getHistoricalPressure(int pos):获取历史数据中第一个手指在第pos事件中的压力大小  
* getHistoricalPressure(int actionIndex, int pos):获取历史数据中第actionIndex个手指在第pos事件中的压力大小  
