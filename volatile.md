```java
int i;
public void setI(int value){
    i = value;
}

public int getI(){
    return i;
}
```

>  对于int型的基本变量，对其赋值操作是原子的，在多线程环境下，对int型值的读写也是能保证原子的。

```java
double i;
public void setI(double value){
    i = value;
}

public double get(){
    return i;
}
```

> 对于double，或者long型的变量，在32位系统下，赋值操作会被拆解为两个操作，会分为高32位和低32位读写，在多线程环境下，不能保证原子性，会出现某个线程B读到线程A才写了一半的操作，也就是"半值"。
>
> * Java虚拟机规范中 没有强制32位系统需要保证double,long型读写的原子性，但是强烈建议保证。
>
> * Java虚拟机规范中，强调了对volatile定义的double,long型读写需要保证原子性。也就是volatile修饰的
>
>   double,long型变量，不会出现读取一半值这种情况。



```java
int i;

public int increase(int i){
    return i++;
} 
```

> 对于上述代码i++这种操作，实际上是典型的read-modify-write操作，这种被称为复合操作，在多线程环境下，会出现与预期结果不符的结果出现。
>
> 这种情况下，推荐使用atomicInteger这种j.u.c包中提供的原子类进行操作。
>
> 顺带说一句，j.u.c并发包的套路基本都是：
>
> * 定义volatile变量
> * 通过cas修改volatile变量