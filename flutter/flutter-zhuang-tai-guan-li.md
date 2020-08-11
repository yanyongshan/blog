# Flutter状态管理:Bloc模式

## Bloc状态管理

### BlocBuilder

BlocBuilder一个特殊的widget,用于响应Bloc事件，将event转化为对应的state,同时可以调用widget的setState来重建widget.

![](../.gitbook/assets/image.png)

```text
BlocBuilder<BlocA, BlocAState>(
  cubit:blocA, //提供BlocA的实例，如果为空将自动使用BlocProvider到当前Context中查询BlocA的实例
  buildWhen: (previousState, state) {
    // 返回true/false决定是否调用builder重建widget
  },
  builder: (context, state) {
    // 根据不同的state,返回不同的widget
  }
)
```

### BlocProvider

BlocProvider用于使当前widget的所有孩子节点都可以通过BlocProvider.of\(context\)获得Bloc实例

```text
BlocProvider(
  lazy: false,
  create: (BuildContext context) => BlocA(), //创建一个新的Bloc
  value: BlocProvider.of<BlocA>(context), //通过父节点中获取，当前Widget关闭后不会关闭父节点的Bloc
  child: ChildA(),
);
```

孩子节点获取状态值

```text
BlocProvider.of<BlocA>(context)或者context.bloc<BlocA>();
```

### **MultiBlocProvider**

当widget需要提供多个Bloc时，可能过创建多个BlocProvider嵌套的方式者是使用MutiBlocProvider

BlocProvider嵌套：

```text
BlocProvider<BlocA>(
  create: (BuildContext context) => BlocA(),
  child: BlocProvider<BlocB>(
    create: (BuildContext context) => BlocB(),
    child: BlocProvider<BlocC>(
      create: (BuildContext context) => BlocC(),
      child: ChildA(),
    )
  )
)

```

MultiBlocProvider:

```text
MultiBlocProvider(
  providers: [
    BlocProvider<BlocA>(
      create: (BuildContext context) => BlocA(),
    ),
    BlocProvider<BlocB>(
      create: (BuildContext context) => BlocB(),
    ),
    BlocProvider<BlocC>(
      create: (BuildContext context) => BlocC(),
    ),
  ],
  child: ChildA(),
)
```

### **BlocListener**

BlocListener用于响应Bloc状态变化，与BlocBuilder主要区别是每个状态仅响应一次（不包括初始状态），如弹窗提示

```text
BlocListener<BlocA, BlocAState>(
  listenWhen: (previousState, state) {
    // return true/false to determine whether or not
    // to call listener with state
  },
  listener: (context, state) {
    // do stuff here based on BlocA's state
  },
  child: Container(),
)
```

