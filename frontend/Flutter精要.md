- [Flutter精要](#flutter精要)
  - [Dart语言](#dart语言)
  - [组件化](#组件化)
  - [状态管理](#状态管理)
    - [StatefulWidget](#statefulwidget)
    - [InheritedWidget](#inheritedwidget)
    - [Provider](#provider)
    - [BLoC（Business Logic Component）](#blocbusiness-logic-component)
    - [Riverpod](#riverpod)
    - [最佳实践](#最佳实践)


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