### final 关键字

先从定义上了解下final关键字

* final关键字可以修饰类，表示该类不允许被继承。
* final关键字可以修饰方法，表示该方法不允许被复写。
* final关键字修饰变量：
  * 修饰基本类型，该变量值不可被修改。
  * 修饰对象引用，只表示该引用不可被修改，但是对象本身行为和属性还是可以修改的。

### immutable

immutable意思是不可变的，Java本身并没有提供原生的不可变支持。满足下列条件对象才是不可变的。

* 对象创建后其状态不能修改。
* 对象属性都是用final修饰的。
* 对象没有发生逃逸。



>推荐使用final关键字来明确表示代码含义，逻辑意图。
>
>- 可以将方法和类声明为final，这样可以明确告诉他人，这些行为不允许修改。
>- final含以上表示不可变，对修饰的属性，不允许被修改，可以避免因为错误的修改导致的程序错误。

> 为什么匿名内部类对外部引用对象需要限制为final？（jdk8后虽然编译器不强制final修饰，但是编译过后，我们还是可以看到编译器帮我们加上了final修饰符）

