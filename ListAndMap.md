List
============
手写ArrayList、LinkeList 和 HashMap(jdk1.7)
----------------------------------
[第一部分](https://github.com/JialongWen/List/tree/master/src/com/wjl/list/arraylist)
-----------
### 手写ArrayList
#### 主要的实现难点<br>
* 主要实现问题在数组扩容
#### 数组扩容问题
* 我们知道数组实质上是一段连续的内存地址
* 而ArrayList是基于数组实现的
* 当数组内存不够时我们该如何扩容
#### 思路
* 为了保持数组内存地址的连续性我想到了直接申请一块新的内存地址将之前的数组内容直接复制到新的内存中
#### 操作方式
* new一个新的数组
* 复制之前的数组到新的数组上（<br>
   数组复制的问题其实我们有很多的选择可以完成这个操作,我们可以使用一个循环例如
   ```Java
   int[] oldArray = new int[10];
   int[] newArray = new int[25];
   for(int i=0; i<oldArray.length; i++){
      newArray[i] = oldArray[i];
   }
   ```
   当然你也可以使用JDK提供给我们的API 例如
   ```Java
   Object[] objects = {1,2};
   Object[] copyNewObjects = Arrays.copyOf(objects,10);
   ```
   当然你也可以使用这个
   ```Java
   int[] fun = {0,1,2,3,4,5}
   System.arraycopy(fun,0,fun,3,3);
   //这个API的有趣之处在于它可以自己复制给自己
   //具体用法请参照JDK文档
   ```
   )
   #### 主要方法的实现
   * add方法的实现
   * get方法的实现
   * remove方法的实现
   * add(int index,E e)添加到具体下标的实现

