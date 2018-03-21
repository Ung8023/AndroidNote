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

```
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
|  byte  | 0(byte) |
| short  | 0(short)|
|  int   |   0   |
| long  | 0L |
| float | 0.0f |
| double | 0.0d |
| char  | '/uoooo'(null) |
| boolean| false|

#### 特殊的变量(context)
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
	2. 在当前的Module包下
	
		```
		<data class=".ContactItem">
		    ...
		</data>
		```
	3. 指定包下
	
		```
		<data class="com.example.ContactItem">
		    ...
		</data>
		```