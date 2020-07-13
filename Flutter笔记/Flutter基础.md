### 入口函数

```dart
void main() => runApp(new app());
通过接收一个widget对象来运行flutter应用。
=> 为单行函数或方法的简写
```

### Widgets

widget是flutter应用的基础，整个应用可以用widget组合而成

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      //应用名称  
      title: 'Flutter Demo', 
      theme: new ThemeData(
        //蓝色主题  
        primarySwatch: Colors.blue,
      ),
      //应用首页路由  
      home: new MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}
```

MyApp 类代表flutter应用，继承了StatelessWidget类，所以其也是一个Widget

Flutter在构建页面时，会调用组件的`build`方法，widget的主要工作是提供一个build()方法来描述如何构建UI界面（通常是通过组合、拼装其它基础widget）。

```dart
class MyHomePage extends StatefulWidget {
  MyHomePage({Key key, this.title}) : super(key: key);
  final String title;
  @override
  _MyHomePageState createState() => new _MyHomePageState();
}
```



StatelessWidget 无状态Widget，即创建后就不能改变
Stateful widget 有状态Widget，创建后是可以变的
Stateful widget至少由两个类组成：一个StatefulWidget类，一个 State类。，StatefulWidget类本身是不变的，但是 State类中持有的状态在widget生命周期中可能会发生变化。







