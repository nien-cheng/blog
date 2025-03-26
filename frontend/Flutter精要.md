- [Flutter精要](#flutter精要)
  - [Dart语言](#dart语言)
  - [组件化](#组件化)
    - [Text](#text)
    - [Image](#image)
    - [Button](#button)
    - [Container](#container)
    - [Row \& Column](#row--column)
    - [Stack](#stack)
    - [TextField](#textfield)
    - [Checkbox \& Radio](#checkbox--radio)
    - [Switch \& Slider](#switch--slider)
    - [Scaffold](#scaffold)
    - [Card](#card)
    - [TabBar \& TabBarView](#tabbar--tabbarview)
  - [状态管理](#状态管理)
    - [StatefulWidget](#statefulwidget)
    - [InheritedWidget](#inheritedwidget)
    - [Provider](#provider)
    - [BLoC（Business Logic Component）](#blocbusiness-logic-component)
    - [Riverpod](#riverpod)
    - [最佳实践](#最佳实践)
  - [动画](#动画)
    - [AnimationController](#animationcontroller)
    - [CurvedAnimation](#curvedanimation)
    - [Tween](#tween)
    - [隐式动画](#隐式动画)
    - [显式动画](#显式动画)
      - [AnimatedBuilder](#animatedbuilder)
      - [AnimatedWidget](#animatedwidget)
      - [PageRouteBuilder](#pageroutebuilder)
  - [渲染](#渲染)
    - [数据结构（三棵树）](#数据结构三棵树)
    - [渲染阶段](#渲染阶段)
    - [性能优化](#性能优化)
  - [Sika引擎](#sika引擎)
    - [分层架构](#分层架构)


# Flutter精要

Flutter作为Google推出的跨平台UI框架。
通过自研的Skia图形引擎绕过原生系统UI组件，直接渲染到设备屏幕，实现“一次编写，多端运行”（支持Android、iOS、Web、桌面端等）。
这种设计避免了传统跨平台框架依赖系统控件导致的性能损耗。

## Dart语言
Flutter是基于Dart语言开发的，Dart是一种面向对象的、函数式的编程语言。
Dart语言具有以下特点：
- 面向对象：Dart语言是一种面向对象的编程语言，支持类、继承、多态等面向对象的特性。
- 函数式：Dart语言支持函数式编程，支持高阶函数、闭包等函数式特性。
- 异步：Dart语言支持异步编程，支持Future、Stream等异步特性。
- 类型安全：Dart语言是一种类型安全的编程语言，支持静态类型检查，避免了类型错误。
- 跨平台：Dart语言可以编译成多种平台的代码，包括Android、iOS、Web、桌面端等。

## 组件化
Flutter采用组件化的设计思想，将界面划分为多个组件，每个组件负责一个功能。
无状态（StatelessWidget）与有状态（StatefulWidget）：前者用于静态内容，后者通过setState()实现动态更新，是构建UI的基础单元。
通过嵌套Widget（如Row、Column、Stack）实现复杂布局，支持高度定制化。

### Text
显示文本内容，支持样式（字体、颜色、对齐等）。
```dart
Text('Hello, Flutter!', style: TextStyle(fontSize: 20, color: Colors.blue))
```

### Image
加载本地或网络图片，支持调整尺寸和填充方式。

```dart
Image.network('https://example.com/image.png', fit: BoxFit.cover)
```

### Button
- ElevatedButton（凸起按钮）
- TextButton（文本按钮）
- FloatingActionButton（悬浮按钮）
- IconButton（图标按钮）

### Container
容器控件，用于包裹子组件并设置布局、装饰（背景色、圆角等）。
```dart
Container(
  width: 100,
  height: 100,
  color: Colors.red,
  child: Text('Container'),
)
```
### Row & Column
分别实现水平和垂直布局，支持对齐方式和间距控制。
```dart
Row(
  children: [Container(width: 50, color: Colors.red), Container(width: 50, color: Colors.blue)],
  mainAxisAlignment: MainAxisAlignment.spaceBetween,
)
```

### Stack
叠加布局，允许子组件重叠，常用于实现复杂UI（如弹窗、图标叠加）。

### TextField
文本输入框，支持输入验证、提示文本等。

### Checkbox & Radio
复选框和单选框，用于多选或单选操作。

### Switch & Slider
开关控件和滑块控件，用于状态切换或数值选择。

### Scaffold
提供Material Design的基础结构，包含导航栏（AppBar）、侧边栏（Drawer）和底部导航栏（BottomNavigationBar）。

### Card
卡片控件，用于展示信息块，带阴影和圆角效果。

### TabBar & TabBarView
选项卡布局，实现多页面切换。

## 状态管理
Flutter的状态管理是构建复杂应用的核心挑战之一，其方案需根据状态作用域、数据复杂度及团队协作需求选择。

### StatefulWidget
原理：通过setState()触发UI重绘，适用于单个Widget的局部状态（如按钮点击计数）。

```dart
class Counter extends StatefulWidget { ... }
class _CounterState extends State<Counter> {
  int count = 0;
  void _increment() => setState(() => count++); // 触发UI更新
}
```
有点事简单直接，但无法跨Widget共享状态。

### InheritedWidget
- 原理：通过Widget树传递数据，实现跨层级状态共享（如主题色、全局配置）。
- 关键方法：dependOnInheritedWidgetOfExactType获取数据，updateShouldNotify控制重建。
- 适用场景：多组件共享同一数据源，但手动维护层级复杂度较高。

### Provider
- 原理：基于InheritedWidget封装，提供轻量级依赖注入，支持ChangeNotifier或Listenable实现状态变更通知。
- 优势：代码简洁，适合中小型应用。
```dart
class Counter with ChangeNotifier {
  int _count = 0;
  int get count => _count;

  void increment() {
    _count++;
    notifyListeners(); // 通知监听者更新UI
  }

  void reset() {
    _count = 0;
    notifyListeners();
  }
}

void main() {
  runApp(
    MultiProvider(
      providers: [
        ChangeNotifierProvider(create: (context) => Counter()),
        // 可添加多个Provider
      ],
      child: MyApp(),
    ),
  );
}
```
在需要使用状态的 Widget 中，通过以下两种方式获取状态：
1. 使用 Provider.of<T>(context) 获取 Provider 实例，然后通过实例访问状态。
```dart
class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final counter = Provider.of<Counter>(context, listen: false); // listen: false 避免重建UI
    return ElevatedButton(
      onPressed: counter.increment,
      child: Text("点击增加"),
    );
  }
}
```
2. 使用 ConsumerWidget 或 Consumer 监听 Provider 的变化，在 build 方法中获取状态。减少不必要的整体重建，提升性能。
```dart
class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Consumer<Counter>(
      builder: (context, counter, child) {
        return Text("当前计数:  $ {counter.count}");
      },
    );
  }
}
```
3. 选择性监听,使用 Selector（需 package:provider ≥6.0.0）优化复杂状态的监听：
```dart
Selector<Counter, int>(
  selector: (context, counter) => counter.count,
  builder: (context, count, child) => Text(count.toString()),
)
```

* Provider的注意事项
  - 避免过度使用 Provider.of：频繁调用可能触发不必要的重建，优先使用 Consumer 或 Selector。
  - 合理设置 listen 参数：在不需要监听变化的场景（如回调函数）中，设置 listen: false。
  - 分层管理状态：将全局状态（如用户信息）放在顶层，局部状态（如表单输入）放在子组件层级。

### BLoC（Business Logic Component）
- 原理：基于事件（Event）-状态（State）流分离业务逻辑，通过Stream实现UI与数据解耦。
- 核心流程：用户操作 → 事件触发 → BLoC处理 → 状态更新 → UI响应。
- 优势：高可测试性、逻辑清晰，适合复杂业务流（如电商、金融类应用）。
- 缺点：代码量较大，学习曲线陡峭。

### Riverpod
- 原理：改进自Provider，支持类型安全、零Context依赖，提供Provider、StateProvider等灵活类型。
- 创新点：
  - 通过ref对象统一管理依赖，避免Provider.of的代码冗余。
  - 支持自动注入与热重载，优化开发体验。

```dart
final counterProvider = StateProvider((ref) => 0);
// 使用ConsumerWidget读取状态
class MyWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final count = ref.read(counterProvider);
    return Text(count.toString());
  }
}
```
优势：官方推荐方案，兼顾性能与易用性，适合中大型项目。

### 最佳实践
- 简单应用：优先使用StatefulWidget或Provider。
- 跨组件共享：选择Provider或Riverpod。
- 复杂业务流：采用BLoC或Riverpod结合StateProvider。
- 最小化状态范围：避免全局状态滥用，仅在必要时使用共享方案。
- 性能优化：使用const和final减少重建，结合ValueListenableBuilder或Consumer实现局部更新。
- 测试支持：BLoC和Riverpod提供内置测试工具，确保逻辑可验证。
- 通过合理选择方案，开发者可构建高效、可维护的Flutter应用，平衡开发效率与代码质量。

## 动画
在 Flutter 中，动画组件体系庞大且灵活，主要分为核心动画控制器、曲线与插值类、隐式动画组件和显示动画组件四大类。

### AnimationController
驱动动画的核心类，控制动画的开始、停止、方向（正向/反向）和时长。
- 关键参数：
  - duration（动画时长）
  - vsync（防抖参数，通常为 SingleTickerProviderStateMixin 的混入类）。
```dart
AnimationController controller = AnimationController(
  duration: Duration(seconds: 2),
  vsync: this,
);
```

### CurvedAnimation
定义动画曲线（如加速、减速），使动画更符合自然运动规律。
- 常用曲线：
  - Curves.linear（匀速）
  - Curves.ease（先加速后减速）
  - Curves.bounceOut（反弹效果）

```dart
CurvedAnimation curve = CurvedAnimation(parent: controller, curve: Curves.easeOut);
```

### Tween
将动画值（0.0～1.0）映射到具体范围（如尺寸、颜色），支持数值、颜色、边距等类型。
```dart
Tween(begin: 100.0, end: 300.0).animate;
ColorTween(begin: Colors.red, end: Colors.blue).animate;
```

### 隐式动画
隐式组件自动管理动画控制器和监听，开发者只需配置起始和结束值，适合简单场景。
```dart
AnimatedContainer(
  duration: Duration(seconds: 1),
  width: expanded ? 200 : 100,
  color: expanded ? Colors.blue : Colors.red,
)

AnimatedOpacity(
  duration: Duration(seconds: 1),
  opacity: visible? 1.0 : 0.0,
  child: Text('Hello, Flutter!'), 
)
```
其他隐式组件还包括：AnimatedAlign、 AnimatedPadding、 AnimatedPhysicalModel、 AnimatedTheme等。

### 显式动画
需结合 AnimationController 和 Listener手动控制，适合复杂动画逻辑。

#### AnimatedBuilder
监听动画值变化，动态构建 UI。
```dart
AnimatedBuilder(
  animation: curve,
  builder: (context, child) => Container(width: 100 + 200 * curve.value),
)
```

#### AnimatedWidget
抽象类，用于封装需监听动画的组件（如 FadeTransition、ScaleTransition）。

- FadeTransition：透明度过渡。
- SlideTransition：平移动画。
- RotationTransition：旋转动画。

#### PageRouteBuilder
自定义页面路由，支持动画效果（如缩放、滑动）。
```dart
Navigator.push(
  context,
  PageRouteBuilder(
    pageBuilder: (context, animation, secondary) => MyPage(),
    transitionsBuilder: (context, animation, secondary, child) => ScaleTransition(scale: curve, child: child),
  ),
);
```

## 渲染
Flutter 通过 Skia 引擎直接渲染到屏幕，不依赖原生控件，解决了混合开发框架（如 React Native）的多端一致性问题，同时保持高性能。
Flutter 的渲染流程分为多个阶段，主要在 UI 线程（Dart）和 GPU 线程（原生引擎）协作完成。

### 数据结构（三棵树）
1. Widget 树
开发者编写的 UI 描述，不可变且轻量级，通过 setState 触发重建。
1. Element 树
Widget 的实例化对象，管理生命周期，协调 Widget 与 RenderObject 的映射。
1. RenderObject 树
实际参与布局和绘制的节点，定义尺寸计算（layout）和绘制逻辑（paint）。

### 渲染阶段
1. 构建阶段（Build Phase）
- 开发者通过 Widget 树描述界面，Flutter 通过 Widget.createElement() 生成 Element 树，管理组件生命周期。
W- idget 是不可变的配置描述，Element 是其实例化对象，负责协调 Widget 与 RenderObject 的关联。
- 优化建议：避免在 build 方法中执行副作用操作，减少嵌套层级以降低遍历节点数量。
1. 布局阶段（Layout Phase）
- RenderObject 树根据父节点约束计算尺寸和位置，形成最终的布局结构。
- 关键机制：
  - Relayout Boundary：通过 RepaintBoundary 等控件划分布局边界，避免父级布局变化影响子级。
  - 尺寸预定义：如 ListView 指定 itemExtent 可减少动态计算开销。
1. 绘制阶段（Paint Phase）
- RenderObject 将布局结果转换为绘制指令，生成 Layer 树，记录绘制操作（如颜色、路径等）。
- Layer 树特性：
  - 每个 Layer 对应特定区域的绘制内容，通过 isRepaintBoundary 标记决定是否局部重绘。
  - RepaintBoundary 用于隔离页面路由或动画区域，减少全局重绘。
1. 光栅化与合成（GPU 线程）
   - Layer 树提交到 Flutter Engine，通过 Skia 渲染引擎进行光栅化（将矢量指令转换为像素），最终合成到屏幕上。

### 性能优化
1. 减少构建与布局开销
   - 使用 const 或 @ immutable 修饰静态 Widget，避免重复创建。
   - 拆分频繁更新的 Widget，使用 StatefulBuilder 局部更新。
2. 高效渲染策略
   - 列表优化：ListView.builder 按需构建可见项，避免一次性渲染全部数据。
   - 避免过度绘制：通过 RepaintBoundary 隔离动画区域，或使用 CustomPaint 直接控制绘制范围。
3. 线程与资源管理
   - Isolate 多线程：处理 CPU 密集型任务（如图片压缩），避免阻塞 UI 线程。
   - GPU 加速：利用 Skia 引擎的硬件加速特性，减少光栅化耗时。

## Sika引擎
Flutter 的渲染引擎基于 Skia 图形引擎，采用 GPU 加速渲染，提供高性能的 UI 绘制能力。
Skia 是一个开源的 2D 图形库，支持文本、路径、位图、图形效果等绘制功能。
其跨平台特性使 Flutter 能在不同操作系统（如 Android、iOS）上生成一致的渲染结果，无需依赖原生 UI 控件。

### 分层架构
- 画布层（Canvas）：提供设备无关的绘图接口，开发者通过 Canvas 调用绘制 API（如 drawRect、drawText）。
- 渲染设备层：根据硬件特性选择 CPU 或 GPU 渲染模式：
  - CPU 渲染：通过内存操作（如 SkBitmapDevice）直接写入像素数据，适用于低端设备或简单场景。
  - GPU 渲染：通过 OpenGL、Vulkan 或 Metal 等底层接口加速，支持复杂图形与高帧率需求。
- 封装适配层：屏蔽不同平台的图形 API 差异，例如通过 GrGLOpsRenderPass 适配 OpenGL。

![architecture](https://docs.flutter.cn/assets/images/docs/arch-overview/archdiagram.png)