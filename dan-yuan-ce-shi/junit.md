## 什么是JUnit框架
是Java下使用最广泛的，最基础的测试框架

## 为什么使用Junit框架
1. 提供java下的单元测试
2. 提供方便的单元测试写法

## 使用Junit框架

[参考文档](https://junit.org/junit4/)

### 添加junit库依赖

```java
dependencies {
    testImplementation 'junit:junit:4.12'
}
```

### 使用`@Test`
在一个测试类中，使用`@Test`来标记一个方法为测试方法。

```java
import junit.framework.Assert;

import org.junit.Test;

public class JavaTestDemoTest {
    @Test
    public void add() throws Exception {
        JavaTestDemo demo = new JavaTestDemo();
        int sum = demo.add(12, 12);
        Assert.assertEquals(24, sum);
    }
}
```

### 使用`@Before`
当我我们在所有的测试方法中都需要调用到某个实例时，可以在`@Before`标记下的方法中对实例进行初始化，
