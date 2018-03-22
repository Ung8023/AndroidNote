### View 与 DataBinding
`View` 的 布局已经通过`DataBinding`创建完成，接下来就要在`Activity`或者`Fragment`中使用DataBinding。

### 获取View
`DataBinding`中生成了 `public final`修饰的View，可以通过`ViewDataBinding(生成的各种子类)`对象直接引用。

### View的行为绑定
如View的点击等等..可以将Listener作为变量，定义在View中，通过`android:onClick`对应内容中使用lambda表达式调用。

