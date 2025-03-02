## Flutter 学习笔记
### Flutter命令
#### 1.查看Flutter运行环境
```
flutter doctor
```
#### 2.获取加载依赖包
```
flutter pub get
```
#### 3.清除构建缓存
```
flutter clean
```
#### 4.更新flutter SDK
```
flutter upgrade
```
#### 5.查看日志
```
flutter logs
```
#### 6.运行程序
```
flutter run -d 设备id
查看设备id：
flutter devices
```
#### 7.构建应用
```
Flutter build apk/ios/web
```
### 快速入门
#### 1.代码结构
* 项目`lib`文件夹存放flutter代码，`main.dart`文件是项目启动文件，`main()`中`runApp()`为启动函数，参数`widget`为主页面widget一般都是`MainApp`widget
* `StatelessWidget`是不涉及事件状态变化最基础widget，`StatefulWidget`是需要涉及事件状态变化的最基础widget
* `StatelessWidget`类型`class`一般需要一个同名构造函数，一个`Widget build(){return Material();}`函数
* `StatefulWidget`类型`class`一般需要一个同名构造函数，一个`createState()`函数创造新的状态类`xxxState()`，`xxState`状态类继承`State<xx>`类，在`xxState`类中去创建`widget`，`Widget build(){}`函数
* 构造函数参数`super.key`是固定参数，函数`{}`表示需要以键值对形式参数传参，`required.this.变量名`表示需要传递的参数
#### 2.使用知识
1. 三元运算符`变量 ？true值 : false值`
2. 容器组件像`Column,Row,Stack`等，可以在**子widget前使用if语句控制子widget的显示**
### Widget组件
#### 1.Expanded
1. 简介：布局类组件，弹性布局，主要搭配Row、Column，按比例扩展子组件所占父组件的控件 
2. 参数：flex（int，默认值1）切割比例值；child子组件
#### 2.Container
1. 简介：容器类组件，主要是设计一些厉害的样式
2. 主要参数：
    * 边框类：margin、padding
    * 尺寸类：height、width、constrains
    * 样式类：decoratin、transform、alignment
    * 子组件：children
#### 3.Column和Row
1. 简介：纵轴和横轴方向堆放排列多个子组件
2. 主要参数：children
#### 4.Scaffold
1. 简介：flutter页面框架的手脚架
2. 主要参数：appbar、body、floatingActionButton. . . 
#### 4.Text
1. 简介：Text为文字组件，支持文字样式，颜色，字体等等改变
2. 主要参数：data、style
#### 5.TextField
1. 简介：文字输入框，结合TextEditingController变量获取输入值
2. 主要参数：controller、decoration（装饰，修饰）
#### 6.IconButton
1. 简介：图标按钮，支持设置不同的图标
2. 主要参数：icon、onPressed等等
### App多语言
#### 第一步 添加依赖库
1.flutter_localizations  
#### 第二步 设置多语言参数
##### 需要写的代码
1. MaterialApp组件
```
一般是MaterialApp（主/顶层widget设置）
localizationsDelegates: [
   // 本地化的代理类
   GlobalMaterialLocalizations.delegate,  // 提供本地化的字符串和其他值
   GlobalWidgetsLocalizations.delegate,  // 定义组件文本默认方向
 ],
 supportedLocales: [
	// 表示我们支持的语言列表
    const Locale('en', 'US'), // 美国英语
    const Locale('zh', 'CN'), // 中文简体
    //其他Locales
  ],
```
2. 手动指定语言
```
locale: const Locale('en','US')
```
##### Localization组件
1. 不是本地化，使用默认语言：Localizations组件用于加载和查找应用当前语言下的本地化值或资源。应用程序通过Localizations.of(context,type) 来引用这些对象
2. 本地化值：本地化值由Localizations的 LocalizationsDelegates 列表加载。每个委托必须定义一个异步load() 方法，以生成封装了一系列本地化值的对象。通常这些对象为每个本地化值定义一个方法