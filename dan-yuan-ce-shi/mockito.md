## 什么是Mock
在做单元测试的时候，我们会发现我们要测试的方法会引用很多外部依赖的对象。 而我们没法控制这些外部依赖的对象。  为了解决这个问题，我们需要用到`Stub`和`Mock`来模拟这些外部依赖的对象,从而控制它们。

## 常见功能

### 验证行为

```java
@Test
public void behaviorVerify() {
    // 创建Mock对象
    List mockList = mock(List.class);

    // 使用Mock对象
    mockList.add("one");
    mockList.clear();

    //验证函数的调用次数
    verify(mockList).add("one");
    verify(mockList).clear();
}
```

### 做测试桩

```java
@Test
public void testStub() {
    //Mock具体的类型
    LinkedList mockList = mock(LinkedList.class);
    
    //添加测试桩,在调用get(0)时返回"first"
    when(mockList.get(0)).thenReturn("a String");
    
    //调用get(1)时抛出异常
    when(mockList.get(1)).thenThrow(new RuntimeException("挂了"));
    
    //输出
    System.out.println(mockList.get(0));
    //抛出异常
    System.out.println(mockList.get(1));
    //输出null，因为没有打桩
    System.out.println(mockList.get(123));
}
```

### 参数匹配器

如果一个方法使用了参数匹配器，就必须所有参数都使用参数匹配器


```java
@Test
public void testParamMatcher() {
    LinkedList mockList = mock(LinkedList.class);

    //使用内置的anyInt()参数匹配器

    when(mockList.get(anyInt())).thenReturn("A");
    //使用自定义的参数匹配器
    when(mockList.contains(myMatcher())).thenReturn(false);

    assertEquals("A", mockList.get(anyInt()));
    assertFalse(mockList.contains(myMatcher()));

}

private String myMatcher() {
    mockingProgress().getArgumentMatcherStorage().reportMatcher((new InstanceOf(String.class, "<aaaaa>")));
    return "";
}
```

#### eq()与anyObject
`eq()`，将传入的参数作为参数
`anyObject()` 返回一个假的值

### 验证函数的确切调用次数、最少调用、从未调用

```java
@Test
public void testMethodTimes() {
    LinkedList mockList = mock(LinkedList.class);

    mockList.add("Once");
    mockList.add("Twice");
    mockList.add("Twice");
    mockList.add("Three times");
    mockList.add("Three times");
    mockList.add("Three times");

    verify(mockList).add("Once");
    verify(mockList, times(1)).add("Once");

    verify(mockList, times(2)).add("Twice");
    verify(mockList, times(3)).add("Three times");

    verify(mockList, never()).add("asdfad");

    // 使用atLeast() | atMost()
    verify(mockList, atLeastOnce()).add("Once");
    verify(mockList, atLeast(2)).add("Three times");
    verify(mockList, atMost(2)).add("Twice");

}
```

### 验证交互操作没有执行在mock对象上

```java
@Test
public void testInteractions() {
    List mock = mock(List.class);

    mock.add("one");

    //普通验证
    verify(mock).add("one");
    // 验证某个交互是否从未被执行
    verify(mock, never()).add("two");
    // 验证mock对象没有交互过
    verifyZeroInteractions(mock);

}
```

### 简化Mock对象创建
通过注解创建对象，当使用`@Mock`注解的对象之前，需要调用`MockitoAnnotations.initMocks()`;

```java
@Mock
private List list;

@Mock
private Map map;

@Before
public void setUp() {
    MockitoAnnotations.initMocks(this);
}

```

### 为连续的调用做测试桩

```java
@Test
public void testLinkStub() {
    List list = mock(List.class);

    when(list.get(0)).thenReturn(30)
            .thenReturn(10)
            .thenReturn(20);

    //等价于：
    when(list.get(1)).thenReturn(10, 20, 30, 40, 50);

    //第一次调用: 返回30
    System.out.println(list.get(0));
    //第二次调用: 返回10
    System.out.println(list.get(0));
    //第三次调用: 返回20
    System.out.println(list.get(0));
}
```

### 为回调做测试桩

```java
@Test
public void testCallBack() {
    MockCallBack mockCallBack = mock(MockCallBack.class);


    MockCallBack.Callback callback = new MockCallBack.Callback() {
        @Override
        public String callBack() {
            return "Zhan";
        }
    };

    when(mockCallBack.doWithCallback(callback)).thenAnswer(new Answer<Object>() {
        @Override
        public Object answer(InvocationOnMock invocation) throws Throwable {
            // 获取函数的调用参数
            Object[] args = invocation.getArguments();
            // 获得Mock对象本身
            Object mock = invocation.getMock();
            return "called with argument:" + args;
        }
    });

    System.out.println(mockCallBack.doWithCallback(callback));
}
```

### do 系列函数
1. `doReturn()`
1. `doThrow()`
1. `doAnswer()`
1. `doNothing()`
1. `doCallRealMethod`

