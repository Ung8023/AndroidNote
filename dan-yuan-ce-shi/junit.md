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
当我我们在所有的测试方法中都需要调用到某个实例时，可以在`@Before`标记下的方法中对实例进行初始化。如果一个方法被`@Before`修饰过了，那么在每个测试方法调用之前，这个方法都会得到调用。

```java
JavaTestDemo demo;

@Before
public void setUp() {
    demo = new JavaTestDemo();
    System.out.println("Init");
}

```

### 使用`@After`
那就是每个测试方法运行结束之后，会得到运行的方法。比如关闭流操作，数据库操作等..

```java
@After
public void release() {
    demo = null;
    System.out.println("release");
}
```

### `@BeforeClass和@AfterClass`

1. `@BeforeClass`的作用是，在跑一个测试类的所有测试方法之前，会执行一次被`@BeforeClass`修饰的方法.
2. 执行完所有测试方法之后，会执行一遍被`@AfterClass`修饰的方法。  
3. 这两个方法可以用来`setup`和`release`一些公共的资源，需要注意的是，被这两个`annotation`修饰的方法必须是静态的。


