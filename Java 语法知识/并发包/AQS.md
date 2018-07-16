**AQS** 全称是AbstractQueuedSynchronizer,从字面意思可以知道是基于Queue实现的同步器，同时是一个抽象类，所以也是整个并发包的基础类。

**AQS**类内部的属性和方法可以简单拆分为：

* 一个volatile 修饰的整形 state 变量

  ```java
  	private volatile int state;
      protected final int getState() {
          return state;
      }
      protected final void setState(int newState) {
          state = newState;
      }
  
  ```





* 一个等待线程队列
* 基于CAS的基础操作方法，以及各种期望具体同步机构去实现得acquire,release方法