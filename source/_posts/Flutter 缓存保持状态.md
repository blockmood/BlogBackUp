---
title: Flutter 缓存保持状态
date: 2019-07-02 15:10:24
tags: Flutter
---

### 根组件实现(必须是StatefulWidget)

案例：

```
class DemoApp extends StatefulWidget with AutomaticKeepAliveClientMixin {
  
  //重点
  @override
  bool get wantKeepAlive => true
  ...省略n多代码
  
  @override
  Widget build(BuildContext context) {
    
    return new MaterialApp(
      title: 'Welcome to Flutter',
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text('Welcome to Flutter'),
        ),
        body: new Center(
          child: new Text('Royce and Owen'),
        ),
      ),
      
    );
  }
}
```

俩句重点语句

> with AutomaticKeepAliveClientMixin

> bool get wantKeepAlive => true

### 导航切换(index)实现

```
...省略
body:IndexedStack(
	index: //当前切换索引
	children: <Widget>组件  注意：这里必须声明Widget组件 否则报错！ find List<Widget> TabLists ...
)
```
