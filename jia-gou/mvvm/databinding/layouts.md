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
2. 可以是`Entity`(实体类)
3. 也可以是其他任意类型

#### 如何理解variable
你可以将它理解为**形参**,