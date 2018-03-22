### 说明
通过Layouts Data Binding，已经基本可以进行开发了。下面是扩展操作.  

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

1. 当表达式写为`@{@color/scrim}`,DataBinding会自动寻找`DrawerLayout`中的`setScrimeColor()`方法
2. 表达式为`@{fragment.drawerListener}`,DataBinding会寻找`DrawerLayout`中的`setDrawerListener()`方法

### 重命名Setters
**不需要开发者重命名，android framework 已经做好了这些工作**

```java
@BindingMethods({
       @BindingMethod(type = "android.widget.ImageView",
                      attribute = "android:tint",
                      method = "setImageTintList"),
})
```

### 自定义Setters
一些属性需要自定义绑定逻辑
#### android 属性已经有对应的BindAdapters
```java
@BindingAdapter("android:paddingLeft")
public static void setPaddingLeft(View view, int padding) {
   view.setPadding(padding,
                   view.getPaddingTop(),
                   view.getPaddingRight(),
                   view.getPaddingBottom());
}
```

#### 案例 ImageView的图片绑定
1.任意类中定义如下：

```java
@BindingAdapter({"bind:imageUrl", "bind:error"})
public static void loadImage(ImageView view, String url, Drawable error) {
   Picasso.with(view.getContext()).load(url).error(error).into(view);
}
```

