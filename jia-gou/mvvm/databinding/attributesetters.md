### 说明
当一个`value`改变时，`binding`必须调用`set`方法来通知**View**更改.**DataBinding**提供一些方式来自定义设置数据方法。

### 自动设置
```xml
<android.support.v4.widget.DrawerLayout
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    app:scrimColor="@{@color/scrim}"
    app:drawerListener="@{fragment.drawerListener}"/>
```

如上：   

1. 当表达式写为`@{@color/scrim}`,DataBinding会自动寻找`DrawerLayout`中的`setScrimeColor()`,方法