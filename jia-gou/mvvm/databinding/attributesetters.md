## Attrbute Setters
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

2.在Layout中引用  

```xml
<ImageView app:imageUrl="@{venue.imageUrl}"
app:error="@{@drawable/venueError}"/>
```

#### 可以为android下的属性写Adapter:

```java
@BindingAdapter("android:paddingLeft")
public static void setPaddingLeft(View view, int oldPadding, int newPadding) {
   if (oldPadding != newPadding) {
       view.setPadding(newPadding,
                       view.getPaddingTop(),
                       view.getPaddingRight(),
                       view.getPaddingBottom());
   }
}
```

#### 事件处理只能通过接口或者只含有一个抽象方法的抽象类

```java
@BindingAdapter("android:onLayoutChange")
public static void setOnLayoutChangeListener(View view, View.OnLayoutChangeListener oldValue,
       View.OnLayoutChangeListener newValue) {
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.HONEYCOMB) {
        if (oldValue != null) {
            view.removeOnLayoutChangeListener(oldValue);
        }
        if (newValue != null) {
            view.addOnLayoutChangeListener(newValue);
        }
    }
}
```

#### 当一个监听器有多个方法时，必须将它分割成多个监听器
比如`View.onAttachStateChangeListener`有两个方法:`onViewAttachedToWindow()`，`onViewDetachedToWindow`,我们需要创建两个接口来区分属性和事件。

```java
@TargetApi(VERSION_CODES.HONEYCOMB_MR1)
public interface OnViewDetachedFromWindow {
    void onViewDetachedFromWindow(View v);
}

@TargetApi(VERSION_CODES.HONEYCOMB_MR1)
public interface OnViewAttachedToWindow {
    void onViewAttachedToWindow(View v);
}
```

因为改变一个监听器会影响另外的监听器，所以我们必须要创建三个BindingAdapters，两个分别对应两种属性，另外一对应为全包含的属性

```java
@BindingAdapter("android:onViewAttachedToWindow")
public static void setListener(View view, OnViewAttachedToWindow attached) {
    setListener(view, null, attached);
}

@BindingAdapter("android:onViewDetachedFromWindow")
public static void setListener(View view, OnViewDetachedFromWindow detached) {
    setListener(view, detached, null);
}

@BindingAdapter({"android:onViewDetachedFromWindow", "android:onViewAttachedToWindow"})
public static void setListener(View view, final OnViewDetachedFromWindow detach,
        final OnViewAttachedToWindow attach) {
    if (VERSION.SDK_INT >= VERSION_CODES.HONEYCOMB_MR1) {
        final OnAttachStateChangeListener newListener;
        if (detach == null && attach == null) {
            newListener = null;
        } else {
            newListener = new OnAttachStateChangeListener() {
                @Override
                public void onViewAttachedToWindow(View v) {
                    if (attach != null) {
                        attach.onViewAttachedToWindow(v);
                    }
                }

                @Override
                public void onViewDetachedFromWindow(View v) {
                    if (detach != null) {
                        detach.onViewDetachedFromWindow(v);
                    }
                }
            };
        }
        final OnAttachStateChangeListener oldListener = ListenerUtil.trackListener(view,
                newListener, R.id.onAttachStateChangeListener);
        if (oldListener != null) {
            view.removeOnAttachStateChangeListener(oldListener);
        }
        if (newListener != null) {
            view.addOnAttachStateChangeListener(newListener);
        }
    }
}
```

## Converters
### Object Conversions
因为在**DataBinding**中，**Layout**中使用 **表达式**来表示，所以返回值有时是**Object**，那么**setter**就会自动匹配类型，并且`Object`会被转成形参需要的类型

```xml
<TextView
   android:text='@{userMap["lastName"]}'
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"/>
```

userMap返回一个对象，而且这个对象会被自动转为`setText(CharSequence)`形参需要的类型。**当参数类型可能混乱时，我们需要在表达式中手动转换**。

### Custom Conversions
当指定类型时，会自动转换

```xml
<View
   android:background="@{isError ? @color/red : @color/white}"
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"/>
```

而因为background的需要的类型是`Drawable`，所以会将`int`型的`color`转为`ColorDrawable`.通过静态方法+`@BindingConversion`，会在找不到自身类中的方法时，寻找带有`@BindingConversion`的方法。

```java
@BindingConversion
public static ColorDrawable convertColorToDrawable(int color) {
   return new ColorDrawable(color);
}
```

因为转换发生来setter方法级别，所以**不能**使用混合类型进行转换  比如:

```xml
<View
   android:background="@{isError ? @drawable/error : @color/white}"
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"/>
```

#### EG
将一个`User`类传入，根据`user`返回不同的`Background`

```xml
<data>
    <variable name="user" type="com.xx.User"/>
</data>

<View
   android:background="@{user}"
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"/>
```

**在View中setBackgroud(User user)方法，所以会寻找带`@BindingConversion`标记的方法**



