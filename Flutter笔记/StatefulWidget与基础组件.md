### StatefulWidget与基础组件

```dart
//StatefulWidget与基础组件
class StateFulGroup extends StatefulWidget {
  @override
  _StateFulGroupState createState() => _StateFulGroupState();
}

class _StateFulGroupState extends State<StatefulWidget> {
  int _currentIndex = 0;

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'StatefulWidget与基础组件',
      theme: ThemeData(primaryColor: Colors.blue),
      darkTheme: ThemeData.dark(),
      home: Scaffold(
        appBar: AppBar(
          title: Text('StatefulWidget与基础组件'),
        ),
        bottomNavigationBar: BottomNavigationBar(
            currentIndex: _currentIndex,
            onTap: (index) {
              setState(() {
                _currentIndex = index;
              });
            },
            items: [
              BottomNavigationBarItem(
                icon: Icon(Icons.android),
                activeIcon: Icon(Icons.android),
                title: Text('首页'),
              ),
              BottomNavigationBarItem(
                icon: Icon(Icons.android),
                activeIcon: Icon(Icons.android),
                title: Text('列表'),
              )
            ]),
        floatingActionButton: FloatingActionButton(
          onPressed: null,
          child: Text('点我'),
        ),
        body: _currentIndex == 0
            ? RefreshIndicator(
                child:ListView(
                  children: <Widget>[
                    Container(
                      decoration: BoxDecoration(color: Colors.blue),
                      alignment: Alignment.center,
                      child: Column(
                        children: <Widget>[Text('第一个')],
                      ),
                    ),
                  ],
                ) ,
                onRefresh: _handleRefresh)
            : Text('第二个'),
      ),
    );
  }
  Future<Null> _handleRefresh() async{
    await Future.delayed(Duration(milliseconds: 200));
    return null;
  }
}

```



MaterialApp:材料设计APP组件

```dart
  const MaterialApp({
    Key key,
    this.navigatorKey,
    this.home,//整个页面 一般scaffold组件组成
    this.routes = const <String, WidgetBuilder>{},
    this.initialRoute,
    this.onGenerateRoute,
    this.onUnknownRoute,
    this.navigatorObservers = const <NavigatorObserver>[],
    this.builder,
    this.title = '',//标题
    this.onGenerateTitle,
    this.color,//主题颜色
    this.theme,//主题
    this.darkTheme,//黑暗主题
    this.locale,
    this.localizationsDelegates,
    this.localeListResolutionCallback,
    this.localeResolutionCallback,
    this.supportedLocales = const <Locale>[Locale('en', 'US')],
    this.debugShowMaterialGrid = false,
    this.showPerformanceOverlay = false,
    this.checkerboardRasterCacheImages = false,
    this.checkerboardOffscreenLayers = false,
    this.showSemanticsDebugger = false,
    this.debugShowCheckedModeBanner = true,
  })
```

