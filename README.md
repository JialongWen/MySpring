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

### 事务的传播行为
#### Spring中事务的定义：<br>
>> Propagation(key 属性确定代理应该给那个方法增加事务行为。这样的属性最重要的部分就是传播行为。）
##### 有以下选项可供使用：
* PROAFGATION_REQUIRED———如果当前有事务，就用`当前事务`,如果当前没有事务，就新建一个事务。这就是最常见的选择。
* PROAFGATION_MANDATORY———支持当前事务，如果当前没有事务，就以非事务方式执行。
* PROAFGATION_REQUITES_NEW———新建事务，如果当前存在事务，吧当前事务挂起。
* PROAFGATION_NOT_SUPPORTED———以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
* PROAFGATION_NEVER———以非事务方式执行，如果当前存在事务，则抛出异常。  
默认传播行为为REQUIRED
