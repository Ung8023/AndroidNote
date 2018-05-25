`Activity`有很多`Flag` ，一些`Flag`可以用来设置启动模式，一些可以用来影响`Activity`的运行状态，这里介绍常见的FLAG

一般通过在`Intent`设置：  

```java
Intent intent = new Intent();
intent.setFlag(XXFLAG | XXFLAG);
```

## 更改启动模式

#### `FLAG_ACTIVITY_NEW_TASK`
相当于
