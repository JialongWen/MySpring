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
   
   [第二部分](https://github.com/JialongWen/List/tree/master/src/com/wjl/list/linkelist)
   ---------
   ### 手写LinkeList
   * LinkeList是基于双向链表实现的
   * 所以我们只需要解决双向链表的特性就可以了
   * 每一个节点都记录他的上一个节点和下一个节点
   * 同时记录首节点和尾节点用来查询和添加
   #### 主要实现方法
   * add 方法的实现
   * get 方法的实现
   * remove方法的实现
   * add(in index,E e)添加到具体下标的方法实现
   
   HashMap
   ========
   ### 手写hashMap
   数组+单链表的形式也就是JDK1.7版本的HashMap JDK1.8版本的是数组+红黑树
   #### 思路或者是需要考虑的点
   * 添加的时候如何解决index冲突（碰撞有时候也叫hash碰撞）
   * 查询的时候如何查询到该下标下是否存在其他节点
   * 何时该扩容如何扩容
   * 扩容时如何解决hash改变的问题
   #### 解决思路或步骤
   * 首先需要解决index冲突的问题，实际上就是分为三个情况<br>
      a.该位置没有列表，那么就是添加新的数据<br>
      b.该位置存在列表，但是列表内没有相同的Key存在（也就是使用equals和==），我们就在该位置列表前添加一个节点（后进先出）<br>
      c.该位置存在列表，并且列表内存在相同的Key，这时候我们就直接替换该节点的value<br>
   * 查询问题我使用的while解决（具体可以查看源码中的getNode()方法）
   ```Java
       Node<K,V> node = node;
        while (nowNode != null){
            //这里我们注意当我们添加第一个的时候就将next中添加为null，当然默认值应该就是null
            //当下一个不存在的时候就直接推出循环返回结果
            node = node.next;
        }
   ```
   * 扩容实际根据源码中的思路，在超过负载容量时就扩容两倍
   * 扩容方法(resize)我的思路同数组扩容时一致直接申请新的数组并将数据迁移过去，但是这里要注意在数组长度改变时key的hash也会改变所以我们需要重新计算index的位置。
   ```Java
    private void resize() {

        //创建新的数组大小为两倍
        Node<K,V>[] newTable = new Node[DEFAULT_INITIAL_SIZE<<1];
        //先将旧的table中的数据取出来重新计算hash
        for (int i = 0; i < DEFAULT_INITIAL_SIZE; i++) {
            Node<K, V> node = table[i];
            //这里我们任然使用while循环来控制判断是否存在下一个
            while (node!=null){
                table[i] = null;//防止资源泄漏
                //计算新的下标
                int index = getIndex(node.getKey(),newTable.length);
                //将他放进去,这里有一点因为可能这个位置之前就值了
                //所以需要将这里的值取出来并且设置为当前的node的下一个再添加到容器中
                Node<K, V> kvNode = newTable[index];
                //取出当前node的下一个备用
                Node<K, V> oldNext = node.next;
                node.next = kvNode;
                newTable[index] = node;
                node = oldNext;
            }
        }
        //循环结束后替换table指向新的table
        //修改各个变量释放不必要的强引用
        table = newTable;
        DEFAULT_INITIAL_SIZE = newTable.length;
        newTable = null;
    }
   ```
   <br>
    `如果您发现有其他问题或者您对我有什么改进的意见欢迎在下方留言或者发送到我的邮箱631488786@qq.com`

