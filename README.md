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


第三部分
--------
### 手写SpringMVC框架流程
>>> SpringMVC原理（执行流程）:<br>
SpringMVC基于Servlet实现
#### @Controller注解的实现
#### 思路:
* 
#### 步骤：
#### @RequestMapping注解
