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

#### 为什么需要导包