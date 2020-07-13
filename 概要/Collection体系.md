### Java集合框架

![image-20200501152735637](E:\Study\笔记\Note\概要\images\image-20200501152735637.png)

![image-20200501153139242](E:\Study\笔记\Note\概要\images\image-20200501153139242.png)





### Map

![image-20200501154426562](E:\Study\笔记\Note\概要\images\image-20200501154426562.png)

HashMap：JDK8以前 数组+链表 数组默认长度16 

​					 JDK8以后  数组+链表+红黑树  容量大于8，元素大于64，则转化为红黑树，否则扩容

![image-20200501155149354](E:\Study\笔记\Note\概要\images\image-20200501155149354.png)

##### HashMap扩容问题：

- 多线程环境下，调整大小会存在条件竞争，容易造成死锁

-  扩容是一个比较耗时的过程

##### Hashtable：

- 早期Java类库提供的哈希表的实现，

- 涉及到修改Hashtable的方法，使用synchronized修饰，线程安全，
- 串行化的方式运行，性能较差

##### 如何优化Hashtable？

- 通过锁细粒度化，将整锁拆解成多个锁进行优化

##### ConccurentHashMap：CAS+synchronized使锁更细化  数组+链表+红黑树

![image-20200501163337904](E:\Study\笔记\Note\概要\images\image-20200501163337904.png)



