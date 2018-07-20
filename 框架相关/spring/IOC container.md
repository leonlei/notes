### 1. IoC 容器 ###

#### 1.1 Ioc容器和beans简介

Ioc也被称为DI（dependency injection）,这是对象定义它们依赖关系的一个过程，也就是它们使用的其它对象。只能通过构造函数参数，工厂方法参数，或者在构造方法，工厂方法返回的实例上设置属性。容器在创建相关bean的时候注入它们的依赖，这个过程与bean本身控制实例化，通过使用类的直接构造来确认其相关依赖项的位置基本是相反的，“控制反转”因此得名。

org.springframework.beans和org.springframework.context包是spring IoC容器的基础，BeanFactory接口提供了一种能够管理任何类型对象的高级配置机制，ApplicationContext是BeanFactory的子接口，它添加了了一些简Spring AOP,消息资源处理（国际化），事件发布更方便的集成，特定用于应用程序层的上下文，如WebApplicationContext用于web应用程序。

简而言之，