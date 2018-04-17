## 什么是JUnit框架
是Java下使用最广泛的，最基础的测试框架

## 为什么使用Junit框架
1. 提供java下的单元测试
2. 提供方便的单元测试写法

## 使用Junit框架

[参考文档](https://junit.org/junit4/)

### JUnit4声明周期

1. 类级初始化资源处理
2. 方法级初始化资源处理
3. 执行测试用例中的方法
4. 方法级销毁资源处理
5. 类级销毁资源处理

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

### `@Ignore`
忽略测试，如在方法还未完善时，可利用此方法.

```java
@Test
public void add() throws Exception {
    int sum = demo.add(12, 12);
    Assert.assertEquals(24, sum);
}
```

### 时间测试
Junit 提供了一个暂停的方便选项。如果一个测试用例比起指定的毫秒数花费了更多的时间，那么 Junit 将自动将它标记为失败。`timeout` 参数和 `@Test` 注释一起使用。

```java
@Test(timeout = 1000)
public void timeOk() {
    System.out.println("time ok");
}

@Test(timeout = 1000)
public void timeFalse() {
    System.out.println("time false");
    while (true);
}
```

### 使用Assert验证结果
```java
assertEquals(expected, actual)
验证expected的值跟actual是一样的，如果是一样的话，测试通过，不然的话，测试失败。如果传入的是object，那么这里的对比用的是equals()

assertEquals(expected, actual, tolerance)
这里传入的expected和actual是float或double类型的，大家知道计算机表示浮点型数据都有一定的偏差，所以哪怕理论上他们是相等的，但是用计算机表示出来则可能不是，所以这里运行传入一个偏差值。如果两个数的差异在这个偏差值之内，则测试通过，否者测试失败。

assertTrue(boolean condition)
验证contidion的值是true

assertFalse(boolean condition)
验证contidion的值是false

assertNull(Object obj)
验证obj的值是null

assertNotNull(Object obj)
验证obj的值不是null

assertSame(expected, actual)
验证expected和actual是同一个对象，即指向同一个对象

assertNotSame(expected, actual)
验证expected和actual不是同一个对象，即指向不同的对象

fail()
让测试方法失败
```

### 异常测试
Junit 用代码处理提供了一个追踪异常的选项。你可以测试代码是否它抛出了想要得到的异常。`expected` 参数和 `@Test` 注释一起使用。

```java
@Test(expected = ArithmeticException.class)
   public void testPrintMessage() { 
      System.out.println("Inside testPrintMessage()");     
      messageUtil.printMessage();     
   }
```

### 套件测试
如果有多个测试类，并且想要一起执行他们`@RunWith` 和 `@Suite` 注释用来运行套件测试。


```java
import org.junit.runner.RunWith;
import org.junit.runners.Suite;
@RunWith(Suite.class)
@Suite.SuiteClasses({
   TestJunit1.class,
   TestJunit2.class
})
public class JunitTestSuite {   
}
```