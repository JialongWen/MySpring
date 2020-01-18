# MySpring
手写Spring部分API
========
[第一部分](https://github.com/JialongWen/Frame.git)  
--------
### 手写Spring事务 ，Spring事务注解，自定义@MyTransaction 实现类似 Spring自带 @Transactional一样功能<br>
#### 思路：<br>
* AOP+自定义注解<br>

#### 步骤：<br>
* 定义事务注解<br>
* 封装手动事务（编码事务）<br>
* 扫包的具体实现<br>
            定义一个事务扫包AOP（具体拦截那些方法？）<br>
* 拦截方法的时候，使用反射技术判断该方法上是否存在事务注解，如果有的话就开启事务，没有的话，就不开启事务。<br>
#### 需要注意的部分：<br>
      在多线程的情况下需要将事务实例设置为prototype（Spring默认为单例）以防止出现现场安全问题。

### 事务的传播行为(暂未实现）
#### Spring中事务的定义：<br>
>> Propagation(key 属性确定代理应该给那个方法增加事务行为。这样的属性最重要的部分就是传播行为。）
##### 有以下选项可供使用：
* PROAFGATION_REQUIRED———如果当前有事务，就用`当前事务`,如果当前没有事务，就新建一个事务。这就是最常见的选择。
* PROAFGATION_MANDATORY———支持当前事务，如果当前没有事务，就以非事务方式执行。
* PROAFGATION_REQUITES_NEW———新建事务，如果当前存在事务，吧当前事务挂起。
* PROAFGATION_NOT_SUPPORTED———以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
* PROAFGATION_NEVER———以非事务方式执行，如果当前存在事务，则抛出异常。  
默认传播行为为REQUIRED


[第二部分](https://github.com/JialongWen/Frame/tree/master/src/main/java/com/wjl/frame/ioc)
--------
### 手写一个IOC容器（基于XML形式，基于注解形式）
#### XML版本思路：dom4j+反射
#### 步骤：
* 解析xml中的信息
* 获取beanId与xml中的节点信息比较
* 通过反射创建实例返回给前者对象

#### 注解版本思路：反射
#### 步骤：
* 使用Java的反射机制扫包，获取当前包下的所有类
* 判断类上是否存在注入bean的注解
* 使用java的反射机制进行初始化

### 依赖注入(Resource)
#### 思路：使用反射机制获取所有属性
#### 步骤：
* 使用反射机制，获取当前类的所有属性
* 判断当前类是否存在注解
* 默认使用属性名称，查找bean容器对象<br>
#### 需要注意的几个点（出现的问题）<br>
* 在IOC容器中存放的应为Objec(实际上Spring也是这么做的）而不是Class<?>这样做是为了后续依赖注入时<br>
导致同样的文件加载两次避免不必要的资源浪费（虽然这么做会提前创建实例占用一定的内存，但是开发者一<br>
般不会将不需要的类存放到IOC容器中（起码我不会这么做））
* 在依赖注入获取bean时由于需要获取类的属性时一定要setAccessible(true)否则会导致私有属性无法赋值
* 依赖注入的进行应在容器初始化之后否则会导致需要注入的类还未加载到容器就已经依赖注入导致注入失效
* 详细问题参考源码注释


[第三部分](https://github.com/JialongWen/MySpringMVC.git)
--------
### 手写SpringMVC框架流程
>> SpringMVC原理（执行流程）:<br>
1.SpringMVC基于Servlet实现。<br>
2.Servlet是单例的，并且Servlet线程不安全。<br>
3.前置控制器(中央控制器）。<br>
#### 整体思路：
* 1.创建一个前端控制器（中央控制器）DispatcherServlet 拦截所有请求（SpringMVC基于Servlet实现）
* 2.初始化操作,实际上就是重写Servlet的init方法。<br>
    2.1 将扫包范围内的所有类，注入到SpringIOC容器(当然他得带有@Controller注解才会被注入）<br>
    2.2 将url映射和方法关联<br>
    2.2.1 判断类上是否存在注解，使用Java反射机制循环遍历方法，判断方法上是否存在注解，进行封装url和方法对应。<br>
* 3.处理请求 重写Get或者是Post方法（实际上doService方法也可以但是这里偷懒了主要是跑一下流程）<br>
  3.1 获取url请求，从urlBeans集合获取实例对象，获取成功实例对象后，调用urlMethods集合获取方法名称，使用反射机制执行。
#### @Controller注解的实现
#### 思路:
* 同IOC一致（实际上应该就是IOC）
#### @RequestMapping注解
#### 思路：
* 反射机制获取到处于IOC容器中的所有对象的属性
#### 步骤：
* 前端控制器实质上是一个Servlet我们在前端控制器init方法内完成IOC容器的初始化和handerMapping的操作。
* 在handerMapping中我将IOC容器中的所有对象都遍历出来，获取他们的方法，并且获取他们方法上的注解以此判断他是不是需要放入到<br>
控制方法的容器中（控制方法容器详情查看源码中的注释，基本写详细了）,也就是完成url映射方法的操作。
* 之后我们再重写他的git,post方法来对请求进行分配（此处SpringMVC应该使用了适配器模式，而我这里没有去过多的关系设计模式，主要目的是为了熟悉SpringMVC的整体流程），当然你也可以修改其他的方法来达到你想要的效果。
* 视图解析部分我只是简单的将内容转发到对应的页面（这里有一个bug至今没有解决）。

