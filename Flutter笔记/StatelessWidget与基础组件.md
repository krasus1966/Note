### StatelessWidget与基础组件

```dart
import 'package:flutter/material.dart';

class LessGroupPage extends StatelessWidget{
  @override
  Widget build(BuildContext context) {
    TextStyle textStyle = TextStyle(fontSize: 20);//设置文本格式
    // TODO: implement build
    return MaterialApp(
      title: 'StatelessWidget与基础组件',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: Scaffold(
        appBar: AppBar(title: Text('StatelessWidget与基础组件'),),
        body: Container(
          decoration: BoxDecoration(color: Colors.white),//装饰器
          alignment: Alignment.center,//文本方式：居中
          child: Column(
            children: <Widget>[
              Text('I am Text',
                style: textStyle,//文字大小20
              ),//文本组件
              Icon(Icons.android,
                size:50,
                color: Colors.red,),
              CloseButton(),//关闭按钮
              BackButton(),//返回按钮
            ],
          ),
        ),
      ),
    );
  }
}
```

Container组件是容器组件

```dart
Container({
    Key key,
    this.alignment,//文本方式
    this.padding,//内边框
    Color color,//颜色
    Decoration decoration,//装饰器
    this.foregroundDecoration,
    double width,//宽度
    double height,//高度
    BoxConstraints constraints,
    this.margin,//外边框
    this.transform,//变形
    this.child,//
  }) 
```

BoxDecoration是装饰器组件

```dart
const BoxDecoration({
    this.color,
    this.image,
    this.border,
    this.borderRadius,
    this.boxShadow,
    this.gradient,
    this.backgroundBlendMode,
    this.shape = BoxShape.rectangle,
  }) 
```

Text是文本框组件

```dart
const Text(this.data, {
    Key key,
    this.style,
    this.strutStyle,
    this.textAlign,
    this.textDirection,
    this.locale,
    this.softWrap,
    this.overflow,
    this.textScaleFactor,
    this.maxLines,
    this.semanticsLabel,
  })
```

Icon是图片组件

```dart
const Icon(this.icon, {
    Key key,
    this.size,
    this.color,
    this.semanticLabel,
    this.textDirection,
  })
```

Chip是圆角卡片组件

```dart
const Chip({
    Key key,
    this.avatar,//头像
    @required this.label,//主体内容
    this.labelStyle,//文字样式
    this.labelPadding,//文字边框
    this.deleteIcon,//
    this.onDeleted,//
    this.deleteIconColor,//
    this.deleteButtonTooltipMessage,//
    this.shape,//
    this.clipBehavior = Clip.none,//
    this.backgroundColor,//
    this.padding,//
    this.materialTapTargetSize,//
    this.elevation,//
  }) 
```

Divide是分割线组件

```dart
const Divider({
    Key key,
    this.height = 16.0,
    this.indent = 0.0,
    this.color
  }) 
```

Card是卡片组件

```dart
const Card({
    Key key,
    this.color,
    this.elevation,
    this.shape,
    this.borderOnForeground = true,
    this.margin,
    this.clipBehavior,
    this.child,
    this.semanticContainer = true,
  })
```

AlertDialog是提示框组件

```dart
const AlertDialog({
    Key key,
    this.title,
    this.titlePadding,
    this.titleTextStyle,
    this.content,
    this.contentPadding = const EdgeInsets.fromLTRB(24.0, 20.0, 24.0, 24.0),
    this.contentTextStyle,
    this.actions,
    this.backgroundColor,
    this.elevation,
    this.semanticLabel,
    this.shape,
  })
```

