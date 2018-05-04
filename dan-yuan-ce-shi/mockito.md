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
