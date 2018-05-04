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