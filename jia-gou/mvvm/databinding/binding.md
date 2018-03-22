### 说明

现在布局已经画好，数据也已经作为"形参"被表示在`Layout`中，那么接下来，就应该对`Binding`，进行操作。根据前面的工作原理图，也就是到了在`Activity或Fragment`中使用**DataBinding**的时候了。

### 创建Binding对象
前面`Layouts`中提到过，可以对`Binding`重新命名，如果没有重新命名，会按照默认规则自动生成，那个类就是我们要使用的类。获取Binding对象有三种方式。

#### 