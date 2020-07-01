# Spring概述

## 什么是spring？

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200506235621.png" style="zoom: 80%;" />







## spring的体系结构

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200507002522.png" alt="image-20200507001906934" style="zoom: 80%;" />







# IoC（Inversion Of Control）的概念和作用

## 程序的耦合

耦合性(Coupling)，也叫耦合度，是对模块间关联程度的度量。模块间的耦合度是指模块之间的依赖关系，包括控制关系、调用关系、数据传递关系。模块间联系越多，其耦合性越强，同时表明其独立性越差( 降低耦合性，可以提高其独立性)。

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200507005756.png" alt="image-20200507004742576" style="zoom: 80%;" />

> Bean对象：在计算机英语中，表示可重用组件的含义。JavaBean表示用Java语言编写的可重用组件。

解决耦合的思路：

1. 尽量<u>**避免使用`new`关键字来创建对象**</u>，而是<u>**通过反射来创建**</u>。
2. <u>**通过读取配置文件来获取创建对象的全限定类名**</u>（包名+类名）。



### 什么是程序耦合

在以前web开发的时候，我们的思路是这样的：

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200509015536.png" alt="image-20200508235443080" style="zoom:80%;" />

这样就出现了耦合，因为每个环节都需要层层依赖。为了避免依赖应该采用工厂模式来解决问题：在实际开发中我们可以**把三层的对象都使用配置文件配置起来**，当启动服务器应用加载的时候，**让一个类中的方法通过读取配置文件，把这些对象创建出来并存起来**。在接下来的使用的时候，直接拿过来用就好了。 那么，这个读取配置文件，创建和获取三层对象的类就是工厂。



### 什么是工厂

工厂就是负责给我们从容器中获取指定对象的类。这时候我们获取对象的方式发生了改变。

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200509015537.png" alt="image-20200509000843542" style="zoom: 67%;" />



### 怎么解决程序耦合（利用工厂模式解耦）

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200509015538.png" alt="image-20200509000944874" style="zoom:67%;" />

也就是说要想解决上面的问题，现在我们需要一个工厂。这个工厂就是来创建我们的service和dao对象的。

1. 需要一个配置文件来配置service和dao的内容。
2. 通过读取配置文件中的配置内容，利用反射创建对象。

> 配置文件可以是XML或者properties文件，spring是通过XML来配置文件的。

java/resource文件下的配置文件如下：

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200509015539.png" alt="image-20200509001624826"  />

test1：（创建工厂）

![image-20200509002939023](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200509015540.png)![image-20200509003813043](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200509015541.png)

---

- [ ] 什么是类加载器
- [ ] 类加载器有什么用
- [ ] 类加载器的执行步骤

---

test1存在的问题：每次调用BeanFactory的时候，都会创建一个AccountServiceImpl或者AccountDaoImpl的实例对象（多例模式），但是这样会导致效率很低，现在我们要求只创建一个这样的对象（单例模式），该怎样创建呢？

[^注意]: tomcat的service也是单例模式，因此在这个模式中，应该尽量避免在类中定义变量。

单例模式创建的实现类都是同一个对象。

多例模式创建的实现类都是不同的对象。

test2：

```java 
public class BeanFactory {
    //定义一个Properties对象
    private static Properties props;

    //定义一个Map,用于存放我们要创建的对象。我们把它称之为容器
    private static Map<String,Object> beans;

    //使用静态代码块为Properties对象赋值
    static {
        try {
            //实例化对象
            props = new Properties();
            //获取properties文件的流对象
            InputStream in = BeanFactory.class.getClassLoader().getResourceAsStream("bean.properties");
            props.load(in);
            //实例化容器,这个容器就是存储实例对象的（AccountServiceImpl、AccountDaoImpl）
            beans = new HashMap<String,Object>();
            //取出配置文件中所有的Key
            Enumeration keys = props.keys();
            //遍历枚举
            while (keys.hasMoreElements()){
                //取出每个Key
                String key = keys.nextElement().toString();
                //根据key获取value
                String beanPath = props.getProperty(key);
                //反射创建对象
                Object value = Class.forName(beanPath).newInstance();
                //把key和value存入容器中
                beans.put(key,value);
            }
        }catch(Exception e){
            throw new ExceptionInInitializerError("初始化properties失败！");
        }
    }

    /**
     * 根据bean的名称获取对象
     * @param beanName
     * @return
     */
    public static Object getBean(String beanName){
        return beans.get(beanName);
    }

```



## IoC的概念和作用

概念：把**创建对象的权力交给框架或者工厂**，它包括<a href="##spring的依赖注入">依赖注入</a>和<a href="#依赖查找">依赖查找</a>。

作用：**削减**计算机程序的**耦合**(解除我们代码中的**依赖**关系)。





# 使用spring的IOC解决程序耦合

## springXML方式的配置

1. 在Maven的pom.xml配置文件中导入spring包

   <img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200509015542.png" alt="image-20200509011949007" style="zoom:80%;" />

2. 在java/resource文件下添加spring约束，并把对象的创建交给spring处理

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <!--把对象的创建交给spring来管理-->
       <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl"></bean>
   
       <bean id="accountDao" class="com.itheima.dao.impl.AccountDaoImpl"></bean>
   </beans>
   ```

   id：获取实例对象的唯一标志。

   class：反射所用到的全限定名。

   spring创建对象的原理：

   ​	spring根据所指引的id，在通过反射构造出class所对应的类。

3. 使用

   ![image-20200509013559295](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200509015543.png)



## 获取核心容器（ApplicationContext）的三种方式

![image-20200509013844418](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200509015544.png)



## BeanFactory和ApplicationContext的区别

![image-20200509015506110](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200509015545.png)



## spring对bean的管理细节

### 创建bean的三种方式

1. 使用默认无参构造函数

![image-20200509152410039](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200509202923.png)

2. ：使用工厂的静态方法创建对象

![image-20200509153036198](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200509202924.png)

3. ：使用工厂的普通方法创建对象

![image-20200509152855265](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200509202925.png)



### bean对象的作用范围

![image-20200509153756512](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200509202926.png)

global-session（常用于集群）：

![image-20200509153913526](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200509202927.png)



### bean对象的生命周期

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200509202928.png" alt="image-20200509161433404" style="zoom:80%;" />





## spring的依赖注入

### 为什么需要依赖注入

spring的IoC就是创建对象的操作交给框架来执行，以此来减少程序的耦合。所谓的依赖注入就是在创建对象的时候给对象一些参数（就像我们创建Java对象时，也需要给对象一些参数再利用`Constuctor()`来创建对象）

### 能注入的数据类型

* 基本类型和String

* 其他bean类型（在配置文件中或者注解配置过的bean）

* 复杂类型/集合类型

  

### 注入的方式

* 使用构造函数提供：使用`<constructor-arg>`标签

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200509202929.png" alt="image-20200509170110525"  />

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200509202930.png" alt="image-20200509170145008" style="zoom:80%;" />

* 使用`set`方法提供

  <img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200509202931.png" alt="image-20200509171226197" style="zoom:80%;" />

  使用`set`方法提供时，其本质是调用了类的`set`方法，因此这个类一定要有`set`方法。比如类有一个方法为`setName`那么这里property中的`name = name`，如果是`setUserName`，那么这里的property中的`name = username`。

  下面使用`set`方法注入发杂型数据（List、Map、Array、Strings[]）示例：

  ​	<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023729.png" alt="image-20200509211752095" style="zoom:80%;" />

  ​	<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023730.png" alt="image-20200509211848931" style="zoom:80%;" />

  ​	<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023731.png" alt="image-20200509211914090" style="zoom:80%;" />

  ​	<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023732.png" alt="image-20200509212042824" style="zoom:80%;" />

  ​	<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023733.png" alt="image-20200509212152686" style="zoom:80%;" />

* 使用注解提供





# spring基于注解的IOC

## spring注解方式的配置

1. 在Maven的pom.xml配置文件中导入spring包

2. 在java/resource文件下添加spring约束，并把对象的创建交给spring处理（这里的配置与XML方式不同）

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context.xsd">
   
       <!--告知spring在创建容器时要扫描的包，配置所需要的标签不是在beans的约束中，而是一个名称为
       context名称空间和约束中-->
       <context:component-scan base-package="com.itheima"></context:component-scan>
   </beans>
   ```

   通过`<context:component-scan>`标签，spring就会扫描该包下所有的包，并找到实现类上面的注解。

## spring中IoC的常用注解

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023734.png" alt="image-20200509214054650" style="zoom:80%;" />

* 用于创建对象的注解

  <img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023735.png" alt="image-20200509215621856" style="zoom:80%;" />

  示例：

  ​	不写value值：<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023736.png" alt="image-20200509215808068" style="zoom:80%;" />

  使用：<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023737.png" alt="image-20200509215933829" style="zoom:80%;" />

  写value值时：<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023738.png" alt="image-20200509220037931" style="zoom:80%;" />

  使用：<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023739.png" alt="image-20200509220116412" style="zoom:80%;" />

* 用于注入数据的注解

  <img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023740.png" alt="image-20200509222242878"  />

1. @Autowird示例：

   ​	![image-20200509222444894](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023741.png)

   ​	Autowired的执行原理：

   ​		如果容器中，只有唯一的bean对象类型要和注入的变量类型匹配，就可以注入成功

   ​		![image-20200509223121810](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023742.png)

   ​		但是如果注入的变量类型在容器中存在两个，那么spring会首先确定匹配的bean对象类型，再匹配变量名

   ![image-20200509223732451](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023743.png)

2.  @Qulifier

   ![image-20200509224628556](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023744.png)

3. @Resource

   ![image-20200509224840556](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023745.png)

* 用于改变作用范围的注解

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023746.png" alt="image-20200509225553844" style="zoom: 67%;" />

* 和生命周期的注解

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023747.png" alt="image-20200509225715364" style="zoom:67%;" />



## :smile:使用XML方式和注解方式实现单表的CURD操作

三层模型图：

![image-20200509233459435](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023748.png)

持久层代码：

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023749.png" alt="image-20200509235040252" style="zoom:50%;" />

业务层代码：

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023750.png" alt="image-20200509235134649" style="zoom:50%;" />

java/resource配置：

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023751.png" alt="image-20200509235815119" style="zoom:80%;" />

完成测试：

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023752.png" alt="image-20200510000118557"  />

---

**利用注解的方式完成上述案例：**

持久层代码：

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023753.png" alt="image-20200510000848561" style="zoom:80%;" />

业务层代码：

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023754.png" alt="image-20200510000945016" style="zoom:80%;" />

java/resource配置：

![image-20200510001115356](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023755.png)

完成测试：

![image-20200510001207046](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023756.png)

在上面的案例中出现了一些问题：

1. 对于第三方包，我们是无法直接加注解的（于是可能想利用纯注解的方式开发）：

   ![image-20200510001115356](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023755.png)

2. 测试类中出现了很多重复的代码，例如​​：

   ![image-20200510020339117](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023801.png)
   
   

**如果我们要想利用纯注解的方式进行开发：**那么我们需要一个配置类

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023805.png" alt="image-20200510022951334" style="zoom:80%;" />

1. 创建一个配置类：

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023757.png" alt="image-20200510001651746" style="zoom:80%;" />

主配置类：

![image-20200510010712545](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023758.png)



子配置类：

```java
public class JdbcConfig {
//用于数据库配置的灵活性，通过读取jdbcConfig.properties配置文件后，利用依赖注入给变量赋值（初始化）
    @Value("${jdbc.driver}")
    private String driver;

    @Value("${jdbc.url}")
    private String url;

    @Value("${jdbc.username}")
    private String username;

    @Value("${jdbc.password}")
    private String password;

    /**
     * 用于创建一个QueryRunner对象
     * @param dataSource
     * @return
     */
    @Bean(name="runner") //将其返回值存放在容器中，并给这个对象起的id为ruuner,通过id就可以找到这个对象
    @Scope("prototype") //多例，因为可能存在多个线程放回，如果使用单例的话，就会造成线程干扰。
    public QueryRunner createQueryRunner(@Qualifier("ds2") DataSource dataSource){
        return new QueryRunner(dataSource);
    }

    
    //如下，出现了两个数据源，而QueryRunner在寻找数据源的时候，首先会进行类型匹配（找到两个），再进行id匹配（如果id都不匹配就会报错），所以上面用@Qulifier来指定数据源使用（通过id来指定）。
    /**
     * 创建数据源对象
     * @return
     */
    @Bean(name="ds2")
    public DataSource createDataSource(){
        try {
            ComboPooledDataSource ds = new ComboPooledDataSource();
            ds.setDriverClass(driver);
            ds.setJdbcUrl(url);
            ds.setUser(username);
            ds.setPassword(password);
            return ds;
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }

    @Bean(name="ds1")
    public DataSource createDataSource1(){
        try {
            ComboPooledDataSource ds = new ComboPooledDataSource();
            ds.setDriverClass(driver);
            ds.setJdbcUrl("jdbc:mysql://localhost:3306/eesy02");
            ds.setUser(username);
            ds.setPassword(password);
            return ds;
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }
}
```

最后使用：

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023800.png" alt="image-20200510014704289" style="zoom:80%;" />





# spring和Junit整合

## Junit无法直接运行spring的原因

![image-20200510014013402](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023759.png)

也就是说，Junit无法直接测试spring是因为再Junit单元测试中无法创建spring核心容器，而要想Junit能够创建核心容器：1. 告诉Junit的spring配置路径。2. 重写Junit的`mian`方法，让他通过spring配置路径创建核心容器。

## 整合方法

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023802.png" alt="image-20200510021741726" style="zoom:80%;" />

示例步骤：

1. 在Maven的pom.xml中导入spring整合junit的依赖：spring-test

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023803.png" alt="image-20200510022257703" style="zoom:80%;" />

2. 用spring提供的@Runwith替换原有junit的`main`方法

   ![image-20200510022511091](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200510023804.png)

3. 告诉Junit，spring创建的方式





# Spring的AOP

## 动态代理的概念

![image-20200510163027009](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200511005320.png)



## 在Java中使用动态代理

### 基于接口的动态代理

![image-20200510220102684](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200511005321.png)

例如：

```java
/**
 * 对生产厂家要求的接口
 */
public interface IProducer {
    public void saleProduct(float money);
}
```

```java
/**
 * 一个生产者
 */
public class Producer implements IProducer{
    public void saleProduct(float money){
        System.out.println("销售产品，并拿到钱："+money);
    }
}
```

```java
        Producer producer = new Producer();
        IProducer proxyProducer = (IProducer) Proxy.newProxyInstance(producer.getClass().getClassLoader(),
                producer.getClass().getInterfaces(),
                new InvocationHandler() {
                    /**
                     * 作用：执行被代理对象的任何接口方法都会经过该方法
                     * 方法参数的含义
                     * @param proxy   代理对象的引用
                     * @param method  当前执行的方法
                     * @param args    当前执行方法所需的参数
                     * @return        和被代理对象方法有相同的返回值
                     * @throws Throwable
                     */
                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        //提供增强的代码
                        Object returnValue = null;

                        //1.获取方法执行的参数
                        Float money = (Float)args[0];
                        //2.判断当前方法是不是销售
                        if("saleProduct".equals(method.getName())) {
                            returnValue = method.invoke(producer, money*0.8f);
                        }
                        return returnValue;
                    }
                });
        proxyProducer.saleProduct(10000f);  //销售产品，并拿到钱：8000
    }
```

- [ ] 去了解Java实现代理的方式



### 基于子类的动态代理

首先，基于子类的动态代理需要第三方包：cglib

![image-20200510221851298](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200511005322.png)

```java
        Producer cglibProducer = (Producer)Enhancer.create(producer.getClass(), new MethodInterceptor() {
            /**
             * 执行被代理对象的任何方法都会经过该方法
             * @param proxy
             * @param method
             * @param args
             *    以上三个参数和基于接口的动态代理中invoke方法的参数是一样的
             * @param methodProxy ：当前执行方法的代理对象
             * @return
             * @throws Throwable
             */
            @Override
            public Object intercept(Object proxy, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
                //提供增强的代码
                Object returnValue = null;

                //1.获取方法执行的参数
                Float money = (Float)args[0];
                //2.判断当前方法是不是销售
                if("saleProduct".equals(method.getName())) {
                    returnValue = method.invoke(producer, money*0.8f);
                }
                return returnValue;
            }
        });
        cglibProducer.saleProduct(12000f);
```



### 详解ClassLoader

#### ClassLoader有什么用？

它是用来加载 Class 的。它**<u>负责将 Class 的字节码形式转换成内存形式的 Class 对象</u>**。字节码可以来自于磁盘文件 *.class，也可以是 jar 包里的 *.class，也可以来自远程服务器提供的字节流。

每个 Class 对象的内部都有一个 classLoader 字段来标识自己是由哪个 ClassLoader 加载的。

```java
class Class<T> {
  ...
  private final ClassLoader classLoader;
  ...
}
```

#### JVM加载的特点 — 延迟加载

JVM 运行并不是一次性加载所需要的全部类的，它是按需加载，也就是延迟加载。程序在运行的过程中会逐渐遇到很多不认识的新类，这时候就会调用 ClassLoader 来加载这些类。加载完成后就会将 Class 对象存在 ClassLoader 里面，下次就不需要重新加载了。

比如你在调用某个类的静态方法时，首先这个类肯定是需要被加载的，但是并不会触及这个类的实例字段，那么实例字段的类别 Class 就可以暂时不必去加载，但是它可能会加载静态字段相关的类别，因为静态方法会访问静态字段。而实例字段的类别需要等到你实例化对象的时候才可能会加载。

#### ClassLoader的分类

JVM 运行实例中会存在多个 ClassLoader，不同的 ClassLoader 会从不同的地方加载字节码文件。它可以从不同的文件目录加载，也可以从不同的 jar 文件中加载，也可以从网络上不同的服务地址来加载。

JVM 中内置了三个重要的 ClassLoader，分别是 BootstrapClassLoader、ExtensionClassLoader 和 AppClassLoader。

BootstrapClassLoader 负责加载 JVM 运行时核心类，这些类位于 JAVA_HOME/lib/rt.jar 文件中，我们常用内置库 java.xxx.* 都在里面，比如 java.util.*、java.io.*、java.nio.*、java.lang.* 等等。这个 ClassLoader 比较特殊，它是由 C 代码实现的，我们将它称之为「根加载器」。

ExtensionClassLoader 负责加载 JVM 扩展类，比如 swing 系列、内置的 js 引擎、xml 解析器 等等，这些库名通常以 javax 开头，它们的 jar 包位于 JAVA_HOME/lib/ext/*.jar 中，有很多 jar 包。

AppClassLoader 才是直接面向我们用户的加载器，它会加载 Classpath 环境变量里定义的路径中的 jar 包和目录。我们自己编写的代码以及使用的第三方 jar 包通常都是由它来加载的。

#### 双亲委派机制

程序在运行过程中，遇到了一个未知的类，它会选择哪个 ClassLoader 来加载它呢？虚拟机的策略是使用调用者 Class 对象的 ClassLoader 来加载当前未知的类。何为调用者 Class 对象？就是在遇到这个未知的类时，虚拟机肯定正在运行一个方法调用（静态方法或者实例方法），这个方法挂在哪个类上面，那这个类就是调用者 Class 对象。前面我们提到每个 Class 对象里面都有一个 classLoader 属性记录了当前的类是由谁来加载的。

待续......

[^reference]:http://blog.itpub.net/31561269/viewspace-2222522/
[^reference]:https://www.jianshu.com/p/554c138ca0f5



## AOP的概念

AOP：全称是Aspect Oriented Programming即：面向切面编程。简单的说它就是把**<u>我们程序重复的代码抽取出来，在需要执行的时候，使用动态代理的技术，在不修改源码的基础上，对我们的已有方法进行增强。</u>**

优势：在程序运行期间，不修改源码对已有方法进行增强。1）减少重复代码、2）提高开发效率、3）维护方便。



## AOP相关术语

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200511005326.png" alt="image-20200510225358334"  />

![image-20200510224254940](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200511005323.png)

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200511005325.png" alt="image-20200510224813655"  />



## spring基于XML配置的AOP

1. 导入必要的依赖包：

![image-20200510224648359](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200511005324.png)

2. 配置spring的环境：xmlns:aop

   示例：在执行AccountServiceImpl对象的所有方法时，都会调用Logger对象的`printLog`方法

   ![image-20200510232316239](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200511005328.png)

   

### XML方式配置AOP的标签

![image-20200510234453717](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200511005330.png)

```xml
<aop:config>:配置管理
<aop:aspect>:配置切面
<aop:before>:前置通知
<aop:after-returning>:后置通知（与异常通知两者只会执行一个）
<aop:after-throwing>:异常通知（与后置通知两者只会执行一个）
<aop:after>:最终通知
<aop:pointcut>:切面表达式,如果写在切面配置的内部，只能作用于当前切面。如果写在管理配置内部（注意只能写在所有切面配置的上面），则作用于所有的切面。
```

示例：

![image-20200510231904870](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200511005327.png)

![image-20200511003943226](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200511005332.png)



环绕通知比较特别这里不做解释（在itheima中也有说明）。

### 切入点表达式

![image-20200511004446810](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200511005333.png)





## spring基于注解配置的AOP

1. 导入必要的依赖包

![image-20200510224648359](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200511005324.png)

2. 配置spring环境：

   ![image-20200510233836450](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200511005329.png)

   

3. 注解使用

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200511005331.png" alt="image-20200510232740042" style="zoom:80%;" />







# spring中的JdbcTemplate

## JdbcTemplate概述

它是spring框架中提供的一个对象，是对原始Jdbc API对象的简单封装。spring框架为我们提供了很多的操作模板类。 操作关系型数据的： JdbcTemplate、HibernateTemplate 操作nosql数据库的： RedisTemplate 操作消息队列的： JmsTemplate。



## JdbcTemplate依赖环境

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200513170854.png" alt="image-20200513165047246" style="zoom:80%;" />

JdbcTemplate在spring-jdbc-5.0.2.RELEASE.jar中，我们在导包的时候，除了要导入这个jar包外，还需要导入一个spring-tx-5.0.2.RELEASE.jar（它是和事务相关的）。



## JdbcTemplate在Dao中的创建

Dao层中的实现：

![image-20200513161306257](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200513170850.png)

将对象的创建交给spring处理：

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200513170848.png" alt="image-20200513155508449" style="zoom:80%;" />

最终使用：

![image-20200513160951641](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200513170849.png)

可能会出现的问题：如果在Dao层不仅仅有AccountDaoImpl，还有ProductDaoImpl、OrderDaoImpl等等Dao实现类，那么就会反复出现这段代码：

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200513170853.png" alt="image-20200513164411845" style="zoom:80%;" />

因为这些Dao的实现类都需要JdbcTemplate对象才能完成创建。那么如何解决这些重复代码呢？:arrow_down_small:（就需要涉及到JdbcDaoSupport类的使用）。

## JdbcDaoSupport类的使用

### 如何使用spring中的JdbcDaoSupport类

![image-20200513161408971](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200513170851.png)

直接通过继承的方式，通过调用父级的`getJdbcTemplate()`来得到JdbcTemplate对象。

spring配置就要做如下更改：

![image-20200513170648950](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200513170855.png)

### JdbcDaoSupport类的源码解析（*原码*）

![image-20200513161710889](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/20200513170852.png)



## JdbcTemplate的CURD操作

![image-20200513172237613](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200513172237613.png)

# spring中的事务控制

spring框架为我们提供了一组事务控制的接口。这组接口是在***<u>spring-tx-5.0.2.RELEASE.jar</u>***中。

## 基于XML的事务控制

1. 导入必要依赖

```xml
    <dependencies>
<!-- spring依赖包 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

<!-- spring JdbcTemplate模板的使用 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

<!-- spring事务支持 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

<!-- spring集成的测试包（必须配合Junit 4.X以上版本使用） -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

<!-- 数据库连接 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>

<!-- 切入点表达式要使用的解析包 -->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.8.7</version>
        </dependency>

<!-- 测试专用，配合与spring集成的spring-test使用时，必须使用junit4.X以上版本 -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
    </dependencies>
```

2. spring配置

![image-20200514160806491](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200514160806491.png)

2. 1配置事务管理器

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200514160059817.png" alt="image-20200514160059817" style="zoom:80%;" />

![image-20200514160849702](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200514160849702.png)

2.2配置事务通知

![image-20200514161423334](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200514160945291.png)

![image-20200514160945291](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200514161345215.png)

2.3配置AOP

![image-20200514161345215](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200514161423334.png)

## 基于注解的事务控制

1. 导入必要依赖

2. spring配置

   ```xml
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xmlns:tx="http://www.springframework.org/schema/tx"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="
           http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/tx
           http://www.springframework.org/schema/tx/spring-tx.xsd
           http://www.springframework.org/schema/aop
           http://www.springframework.org/schema/aop/spring-aop.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context.xsd">
   
       <!-- 配置spring创建容器时要扫描的包-->
       <context:component-scan base-package="com.itheima"></context:component-scan>
   
       <!-- 配置JdbcTemplate-->
       <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
           <property name="dataSource" ref="dataSource"></property>
       </bean>
   
   
   
       <!-- 配置数据源-->
       <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
           <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
           <property name="url" value="jdbc:mysql://localhost:3306/eesy"></property>
           <property name="username" value="root"></property>
           <property name="password" value="1234"></property>
       </bean>
   
       <!-- spring中基于注解 的声明式事务控制配置步骤
           1、配置事务管理器
           2、开启spring对注解事务的支持
           3、在需要事务支持的地方使用@Transactional注解
   
   
        -->
       <!-- 配置事务管理器 -->
       <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
           <property name="dataSource" ref="dataSource"></property>
       </bean>
   
   
   
       <!-- 开启spring对注解事务的支持-->
       <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>
   
   </beans>
   ```

   Dao实现类：

   <img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200514162101122.png" alt="image-20200514162101122" style="zoom:80%;" />

   Service实现类：

   <img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200514162301729.png" alt="image-20200514163734587" style="zoom:80%;" />

   



## 纯注解式的事务控制

1. 导入必要依赖

2. 建立一个配置类

   JdbcConfig:

   ```java
   public class JdbcConfig {
   
       @Value("${jdbc.driver}")
       private String driver;
   
       @Value("${jdbc.url}")
       private String url;
   
       @Value("${jdbc.username}")
       private String username;
   
       @Value("${jdbc.password}")
       private String password;
   
       /**
        * 创建JdbcTemplate
        * @param dataSource
        * @return
        */
       @Bean(name="jdbcTemplate")
       public JdbcTemplate createJdbcTemplate(DataSource dataSource){
           return new JdbcTemplate(dataSource);
       }
   
       /**
        * 创建数据源对象
        * @return
        */
       @Bean(name="dataSource")
       public DataSource createDataSource(){
           DriverManagerDataSource ds = new DriverManagerDataSource();
           ds.setDriverClassName(driver);
           ds.setUrl(url);
           ds.setUsername(username);
           ds.setPassword(password);
           return ds;
       }
   }
   ```

   TransactionConfig：

   ```java
   /**
    * 和事务相关的配置类
    */
   public class TransactionConfig {
   
       /**
        * 用于创建事务管理器对象
        * @param dataSource
        * @return
        */
       @Bean(name="transactionManager")
       public PlatformTransactionManager createTransactionManager(DataSource dataSource){
           return new DataSourceTransactionManager(dataSource);
       }
   }
   ```

   SpringConfiguration：

   ```java
   /**
    * spring的配置类，相当于bean.xml
    */
   @Configuration
   @ComponentScan("com.itheima")
   @Import({JdbcConfig.class,TransactionConfig.class})
   @PropertySource("jdbcConfig.properties")
   @EnableTransactionManagement
   public class SpringConfiguration {
   }
   ```

3. 测试：

   ```java
   /**
    * 使用Junit单元测试：测试我们的配置
    */
   @RunWith(SpringJUnit4ClassRunner.class)
   @ContextConfiguration(classes= SpringConfiguration.class)
   public class AccountServiceTest {
   
       @Autowired
       private  IAccountService as;
   
       @Test
       public  void testTransfer(){
           as.transfer("aaa","bbb",100f);
       }
   }
   ```

目录层级：

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200514163734587.png" alt="image-20200514162301729" style="zoom:80%;" />