

### 1. IoC 容器 ###

#### 1.1 Ioc容器和beans简介

Ioc也被称为DI（dependency injection）,这是对象定义它们依赖关系的一个过程，也就是它们使用的其它对象。只能通过构造函数参数，工厂方法参数，或者在构造方法，工厂方法返回的实例上设置属性。容器在创建相关bean的时候注入它们的依赖，这个过程与bean本身控制实例化，通过使用类的直接构造来确认其相关依赖项的位置基本是相反的，“控制反转”因此得名。

org.springframework.beans和org.springframework.context包是spring IoC容器的基础，BeanFactory接口提供了一种能够管理任何类型对象的高级配置机制，ApplicationContext是BeanFactory的子接口，它添加了了一些简Spring AOP,消息资源处理（国际化），事件发布更方便的集成，特定用于应用程序层的上下文，如WebApplicationContext用于web应用程序。

简而言之，BeanFactory提供框架配置和基本功能，ApplicationContext添加了更多企业定制功能，ApplicationContext是BeanFactory的完全超集。



#### 1.2 容器概述 ####

ApplicationContext作为Spring IoC容器代表，负责实例化，配置，组装beans。容器通过读取配置元数据获取有关要实例化，配置和组装的对象的指令。配置元数据可以通过XML,Java注解，或者Java代码。你可以通过它描述应用组成应用程序的对象以及这些对象之间丰富的相互依赖关系。

Springt提供了许多ApplicationContext拆箱即用的接口实现。

在大多数应用程序中，用户代码不需要显式的实例化Sring Ioc容器，例如，在Web应用程序中，web.xml文件中简单的几行的XML描述符就足够了。下图是spring IoC容器工作原理的高度概括。

![container magic](https://docs.spring.io/spring/docs/current/spring-framework-reference/images/container-magic.png)

##### 1.2.1 元数据配置

Spring IoC容器使用一种配置元数据，该元数据配置表示您作为应用程序开发人员告诉Spring容器如何在应用程序中实例化，配置和组装对象。

元数据配置的几种方式：

* 传统的XML配置
* Java注解 （annotation-based-configuration）
* Java code （Java-based-configuration）



#### 1.3 Bean 概述

spring容器管理一个或多个bean,这些bean根据你提供给容器的配置元数据创建的，比如 XML配置文件的<bean/>定义。

在容器内，这些bean的定义表示为BeanDefinition对象，其中包含（以及其他信息）以下元信息：

* 包限定的类名: 通常是定义bean的实际实现类。
* Bean行为配置元素，说明bean在容器中的行为方式（范围，生命周期回调等）。
* bean执行其工作所需的其他bean的引用，这些引用也称为协作者或依赖项。
* 要在新创建的对象中设置的其他配置设置，例如，在管理连接池的Bean中使用的连接数，或池的大小限制。

| Property                 | Explained in…                                                |
| ------------------------ | ------------------------------------------------------------ |
| class                    | [Instantiating beans](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-class) |
| name                     | [Naming beans](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-beanname) |
| scope                    | [Bean scopes](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-scopes) |
| constructor arguments    | [Dependency Injection](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-collaborators) |
| properties               | [Dependency Injection](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-collaborators) |
| autowiring mode          | [Autowiring collaborators](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-autowire) |
| lazy-initialization mode | [Lazy-initialized beans](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-lazy-init) |
| initialization method    | [Initialization callbacks](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-lifecycle-initializingbean) |
| destruction method       | [Destruction callbacks](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-lifecycle-disposablebean) |

除了包含有关如何创建特定bean的信息的bean定义之外，ApplicationContext实现还允许用户注册在容器外部创建的现有对象。这是通过getBeanFactory（）方法访问ApplicationContext的BeanFactory来完成的，该方法返回BeanFactory实现DefaultListableBeanFactory。DefaultListableBeanFactory通过方法registerSingleton（..）和registerBeanDefinition（..）支持此注册。但是，典型应用程序仅适用于通过元数据bean定义定义的bean。



##### 1.3.1 命名Beans

##### 1.3.2 实例化 beans

bean定义本质上是用于创建一个或多个对象的配方。容器在被调用时，查看该命名bean的配方，并使用由该bean定义封装的配置元数据来创建（或获取）实际对象。

* 通常，在容器本身通过反向调用其构造函数直接创建bean的情况下指定要构造的bean类，稍微等同于使用new运算符的Java代码。
* 要指定包含将被调用以创建对象的静态工厂方法的实际类，在不太常见的情况下，容器在类上调用静态工厂方法来创建bean。从静态工厂方法的调用返回的对象类型可以完全是同一个类或另一个类。

###### 通过构造方法实例化

当您通过构造方法创建bean时，所有普通类都可以使用并与Spring兼容。也就是说，正在开发的类不需要实现任何特定接口或以特定方式编码。简单地指定bean的类就足够了。但是，根据您为该特定bean使用的IoC类型，您可能需要一个默认（空）构造函数

Spring IoC容器几乎可以管理您希望它管理的任何类;它不仅限于管理真正的JavaBeans。大多数Spring用户更喜欢实际的JavaBeans，只有一个默认（无参数）构造函数，并且在容器中的属性之后建模了适当的setter和getter。您还可以在容器中拥有更多异国情调的非bean样式类。例如，如果您需要使用绝对不符合JavaBean规范的旧连接池，那么Spring也可以对其进行管理。

使用基于XML的配置元数据，您可以按如下方式指定bean类：

```XML
<bean id="exampleBean" class="examples.ExampleBean"/>

<bean name="anotherExample" class="examples.ExampleBeanTwo"/>
```

###### 使用静态工厂方法实例化

```xml
<bean id="clientService"
    class="examples.ClientService"
    factory-method="createInstance"/>
```

```java
public class ClientService {
    private static ClientService clientService = new ClientService();
    private ClientService() {}

    public static ClientService createInstance() {
        return clientService;
    }
}
```

###### 使用实例工厂实例化

```xml
<!-- the factory bean, which contains a method called createInstance() -->
<bean id="serviceLocator" class="examples.DefaultServiceLocator">
    <!-- inject any dependencies required by this locator bean -->
</bean>

<!-- the bean to be created via the factory bean -->
<bean id="clientService"
    factory-bean="serviceLocator"
    factory-method="createClientServiceInstance"/>
```

```java
public class DefaultServiceLocator {

    private static ClientService clientService = new ClientServiceImpl();

    public ClientService createClientServiceInstance() {
        return clientService;
    }
}
```

一个工厂类可以hold多个工厂方法，如下：

```xml
<bean id="serviceLocator" class="examples.DefaultServiceLocator">
    <!-- inject any dependencies required by this locator bean -->
</bean>

<bean id="clientService"
    factory-bean="serviceLocator"
    factory-method="createClientServiceInstance"/>

<bean id="accountService"
    factory-bean="serviceLocator"
    factory-method="createAccountServiceInstance"/>
```

```java
public class DefaultServiceLocator {

    private static ClientService clientService = new ClientServiceImpl();

    private static AccountService accountService = new AccountServiceImpl();

    public ClientService createClientServiceInstance() {
        return clientService;
    }

    public AccountService createAccountServiceInstance() {
        return accountService;
    }
}
```

