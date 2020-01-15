# MySpring
手写Spring部分API
========
第一部分  
--------
### 手写Spring事务 ，Spring事务注解，自定义@MyTransaction 实现类似 Spring自带 @Transactional一样功能<br>
#### 思路：<br>
AOP+自定义注解<br>
####步骤：<br>
a.定义事务注解<br>
      b.封装手动事务（编码事务）<br>
      c.扫包的具体实现<br>
            定义一个事务扫包AOP（具体拦截那些方法？）<br>
      d.拦截方法的时候，使用反射技术判断该方法上是否存在事务注解，如果有的话就开启事务，没有的话，就不开启事务。<br>
需要注意的部分：<br>
      在多线程的情况下需要将事务实例设置为prototype（Spring默认为单例）以防止出现现场安全问题。<br>
