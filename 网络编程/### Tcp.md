### Tcp

##### 1.客户端

```JAVA
/**
  * 创建Socket连接服务器（指定ip地址，端口号）通过ip地址找对应的服务器
  * 调用Socket的getInputstream()和getOutputStream()方法获取和服务器相连的IO流
  * 输入流可以读取服务端输出流写出的数据
  * 输出流可以写出数据到服务端的输入流
**/
public static void main(String[] args){
    Socket socket = new Socket("ip地址",端口号);
    //获取客户端输入流
    InputStream is = socket.getInputStream();
    //获取客户端输出流
    OutputStream os = socket.getOutputStream();
    
}

```

##### 2.服务端

```JAVA
/**
  * 创建ServerSocket（需要指定端口号）
  * 调用ServerSocket的accept()方法接收一个客户端请求，得到一个Socket
  * 调用Socket的getInputStream()和getOutputStream()方法获取和客户端相连的IO流
  * 输入流可以读取客户端输出流写出的数据
  * 输出流可以写出数据到客户端的输入流
**/
public static void main(String[] args){
    
}
```

