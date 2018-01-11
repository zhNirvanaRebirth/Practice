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
