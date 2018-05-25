`Activity`有很多`Flag` ，一些`Flag`可以用来设置启动模式，一些可以用来影响`Activity`的运行状态，这里介绍常见的FLAG

一般通过在`Intent`设置：  

```java
Intent intent = new Intent();
intent.setFlag(XXFLAG | XXXFLAG);
```

## 更改`Activity`启动模式

### `FLAG_ACTIVITY_NEW_TASK`
相当于在`xml`中指定`singleTask`模式：  

```java
intent.setFlag(Intent.FLAG_ACTIVITY_NEW_TASK);
```

### `FLAG_ACTIVITY_SINGLE_TOP`
相当于在`xml`中指定`singleTop`模式：  

```java
intent.setFlag(Intent.FLAG_ACTIVITY_SINGLE_TOP);
``` 

## 更改`Activity`运行状态

### `FLAG_ACTIVITY_CLEAR_TOP`
具有此标记位的`Activity`在同一个任务栈中，所有位于它上面的`Activity`都要出栈

#### 与`FLAG_ACTIVITY_NEW_TASK`配合使用
相当于在XML中指定SingleTask``