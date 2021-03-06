### View工作原理
![](/assets/View工作原理.png)
### View 与 DataBinding
`View` 的 布局已经通过`DataBinding`创建完成，接下来就要在`Activity`或者`Fragment`中使用DataBinding。

### 获取View
`DataBinding`中生成了 `public final`修饰的View，可以通过`ViewDataBinding(生成的各种子类)`对象直接引用。

### View的行为绑定
如View的点击等等..可以将Listener作为变量，定义在View中，通过`android:onClick`对应内容中使用lambda表达式调用。

```xml
<data>
    <variable name="listener" type="com.xx.ClickListener" />
    <variable name="data" type="com.xx.Data" />

    <View 
    ...
    android:onClick="@{() -> listener.onClick()}"
    //或者带参数
    android:onClick="@{view -> listener.onClick(view, data)}"
    />
</data>
```

```java
public interface ClickListener {
    void onClick(View view, Data data);
}
```

### 逻辑处理
在View中，通过Controller,Presenter或者ViewModel处理逻辑，逻辑结束，将结果返回到`Activity/Fragment`，通知View进行更新。当然如果使用观察者模式，可以在数据变化时，自动更新UI，最终原理是一样的
