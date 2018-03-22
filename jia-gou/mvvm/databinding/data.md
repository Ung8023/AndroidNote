### 说明

既然已经对`Layout`做出了更改，而`Layout`中又引用了`POJO`，或者`ViewModel` ，那就不得不提，**DataBinding **提供的新式的定义Object的方式，可观察变量的方式，通过这种方式，可以达到当数据改变时，UI可以自动刷新，实现原理是**观察者模式**

### 普通定义POJO的方式

就像在`Layouts` 中介绍的那中引入变量时，任何Class都可以作为一个变量被引入，所以，如果没有对**数据驱动UI改变**有要求的话，可以按原来的方式定义POJO。

1. 定义POJO

    ```java
    public class User {
        private int age;
        private String name;
        
        getters and setters
    }
    ```

2. 使用POJO
    
    ```java
    <data>
        <variable name="user" type="com.sample.User"/>
    </data>
    ```

### 定义可观察的Object

然而实际上，**DataBinding**提供的数据驱动UI，是一个强有力的帮手，**当数据变化，直接对UI进行更改**，而最本质的实现机理，就是观察者模式。
