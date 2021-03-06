### 说明

使用`DataBinding`，带来的第一个改变就是**Layout**上的变化，要对`Layout`进行改造。

### 创建新式Layout

#### 新的结构

##### 原来的Layout结构：

```xml
<LinearLayout>
    <CheckBox/>
    <TextView/>
</LinearLayout>
```

##### DataBinding的Layout结构

```xml
<layout>
    <data>
        <import />
        <variable />
    </data>

    <LinearLayout>
    <CheckBox/>
    <TextView/>
    </LinearLayout>
</layout>
```

#### DataBinding Layout结构说明

1. 跟布局变为`<layout>`
2. 原来的布局原样作为`<layout>`的子节点
3. 增加了`<data>`节点，可以用来导包，或者定义变量

#### 为什么需要导包？

在`DataBinding` 下的`xml`，可以直接使用类的成员，比如`View`的可见属性等，但是这些包在`xml`中，是无法直接使用的，需要像写`Java`代码一样，将对应的类导入。

#### variable中定义的到底是什么？

1. 可以是`ViewModel`
2. 可以是`Entity`\(实体类\)
3. 也可以是其他任意类型

#### 如何理解variable

可以将它理解为`xml`里的**形参**，它会作为成员被生成到DataBinding中。

### Imports

可以在`<data>`节点下进行导包，与使用`java`一样，对于想使用的包,要进行导入,以方便访问。

1. 像java一样导入一个包或者类

   ```xml
    <data>
        <import type="android.view.View"/>
    </data>

    <TextView
       android:text="@{user.lastName}"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:visibility="@{user.isAdult ? View.VISIBLE : View.GONE}"/>
   ```

2. 指定别名

   ```xml
    <import type="android.view.View"/>
    <import type="com.example.real.estate.View"
        alias="Vista"/>
   ```

3. 可以直接使用静态属性

4. `java.lang.*`下的已经自动导入

### 变量

变量是可以被认为是，在`Layout`中引用时候的形参，在编译过程中，会作为私有成员封装在生成的Binding文件中。

```xml
<data>
    <import type="android.graphics.drawable.Drawable"/>
    <variable name="user"  type="com.example.User"/>
    <variable name="image" type="Drawable"/>
    <variable name="note"  type="String"/>
</data>
```

#### 变量的默认值

如果定义的变量**没有设置**，那么将会是默认值，比如：

| 变量类型 | 默认值 |
| :---: | :---: |
| 引用类型 | null |
| byte | 0\(byte\) |
| short | 0\(short\) |
| int | 0 |
| long | 0L |
| float | 0.0f |
| double | 0.0d |
| char | '/uoooo'\(null\) |
| boolean | false |

#### 特殊的变量\(context\)

name为`context`的自动生成，值来自于**rootView**的`getContext()`方法，如果定义了与`context`同名的变量，则原来的context就会被替换。

### 定义Binding类名

#### 默认

1. 根据Layout的名字生成
2. 生成规则：按照类名规则,去掉下划线末尾加上Binding

#### 自定义Binding的类名

* 方式：在指定data的时候，通过class属性指定类名
* 三种类名定义  
    1. 在databinding生成的包下

  ```
        <data class="ContactItem">
            ...
        </data>
  ```

  1. 在当前的Module包下

     ```
      <data class=".ContactItem">
          ...
      </data>
     ```

  2. 指定包下

     ```
      <data class="com.example.ContactItem">
          ...
      </data>
     ```

### include

1. 可以传递变量到`include` 的`Layout`中
2. 但是`include`的`layout`中必须含有，对应的变量

   ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <layout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:bind="http://schemas.android.com/apk/res-auto">
       <data>
           <variable name="user" type="com.example.User"/>
       </data>
       <LinearLayout
           android:orientation="vertical"
           android:layout_width="match_parent"
           android:layout_height="match_parent">
           <include layout="@layout/name"
               bind:user="@{user}"/>
           <include layout="@layout/contact"
               bind:user="@{user}"/>
       </LinearLayout>
    </layout>
   ```

3. DataBinding 不支持将`include`的`layout`通过`merge`标签作为直接子节点。例如以下方式是**不支持的**

   ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <layout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:bind="http://schemas.android.com/apk/res-auto">
       <data>
           <variable name="user" type="com.example.User"/>
       </data>
       <merge>
           <include layout="@layout/name"
               bind:user="@{user}"/>
           <include layout="@layout/contact"
               bind:user="@{user}"/>
       </merge>
    </layout>
   ```

### 支持的表达式

#### 普通表达式

* 数学 `+ - / * %`
* 字符串拼接 `+`
* 逻辑表达式 `&& ||`
* 位运算 `& | ^`
* 一元运算符 `+ - ! ~`
* 移位 `>> >>> <<`
* 比较 `== > < >=  <=`
* instanceof
* 分组`()`
* 字符
* 类型转换
* 调用方法
* 访问属性
* 数组访问 `[]`
* 三元运算符 `?:`

#### 空合并表达式

```xml
android:text="@{user.displayName ?? user.lastName}"
```

等价于

```xml
android:text="@{user.displayName != null ? user.displayName : user.lastName}"
```

#### 属性引用

引用属性时，是根据属性名称访问，要么属性是`public`修饰，要么给属性提供`getter 和 setter`

```xml
android:text="@{user.lastName}"
```

### 避免空指针

1. 如果引用对象, ${user.name}

   * name为null  -&gt; 则表达式为null
   * user为null  -&gt; 则表达式为null

2. 引用${user.age}，age为int

   * user为null -&gt; 表达式为0

### 引用文字

```xml
android:text='@{map["firstName"]}' 
android:text="@{map[`firstName`}"  
android:text="@{map['firstName']}"
```

### 引用集合

使用集合时，如果定义带泛型的集合需要使用`&lt; 与 &gt; 分别代表< 与 >`,访问数据时，通过`[]`访问：

```xml
<data>
    <import type="android.util.SparseArray"/>
    <import type="java.util.Map"/>
    <import type="java.util.List"/>
    <variable name="list" type="List&lt;String&gt;"/>
    <variable name="sparse" type="SparseArray&lt;String&gt;"/>
    <variable name="map" type="Map&lt;String, String&gt;"/>
    <variable name="index" type="int"/>
    <variable name="key" type="String"/>
</data>
…
android:text="@{list[index]}"
…
android:text="@{sparse[index]}"
…
android:text="@{map[key]}"

```

### 引用资源

#### 直接引用

```xml
android:padding="@{large? @dimen/largePadding : @dimen/smallPadding}"
```

#### 引用字符串资源

```xml
android:text="@{@string/nameFormat(firstName, lastName)}"
android:text="@{@plurals/banana(bananaCount)}"
```

复数字符串:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <plurals
        name="banana">
        <item quantity="one">have a banana</item>
        <item quantity="other">have %d bananas</item>
    </plurals>
</resources>
```

如复数字符串资源需要多个参数,需要传递多个参数

```xml
android:text="@{@plurals/orange(orangeCount, orangeCount)}"
```

#### 一些资源需要指定明确的引用:

| 类型 | 普通引用 | DataBinding表达式引用 |
| :---: | :---: | :---: |
| String\[\] | @array | @stringArray |
| int\[\] | @array | @intArray |
| TypedArray | @array | @typedArray |
| Animator | @animator | @animator |
| StateListAnimator | @animator | @stateListAnimator |
| color int | @color | @color |
| ColorStateList | @color | @colorStateList |



