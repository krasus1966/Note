### String，StringBuffer，StringBuilder的区别

String：

StringBuffer：线程安全，节省空间

StringBuilder：线程不安全，高效



### Java异常

What：异常类型回答了什么被抛出

Where：异常堆栈跟踪回答了在哪抛出

Why：异常信息回答了为什么被抛出

![image-20200430163913534](E:\Study\笔记\Note\概要\images\image-20200430163913534.png)

Error：程序无法处理的系统错误，编译器不做检查

Exception：程序可以处理的异常，可以被捕获，可能被恢复

RuntimeException：不可预知的，程序应当自行避免

非RuntimeException：可预知的，从编译器校验的异常



从责任角度看：

1. Error属于JVM需要承担的责任；
2. RuntimeException是程序应该负担的责任；
3. Checked Exception可检查异常是Java编译器应该负担的责任。







