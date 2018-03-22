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

然而实际上，**DataBinding**提供的数据驱动UI，是一个强有力的帮手，**当数据变化，直接对UI进行更改**，而最本质的实现机理，就是观察者模式。而**DataBinding**提供给**Data**，来实现这种变化的有三种：  

* Observable Objects
* Observable fields
* Observable Collections

#### Observable Objects
制作`Observable Objects`有两种方式:

1. 实现 `android.databinding.Observable`接口
2. 继承 `android.databinding.BaseObservable` 类(此类是`DataBinding`提供的一个已经对`Observable接口`基础实现的类)

```java
private static class User extends BaseObservable {
   private String firstName;
   private String lastName;
   @Bindable
   public String getFirstName() {
       return this.firstName;
   }
   @Bindable
   public String getLastName() {
       return this.lastName;
   }
   public void setFirstName(String firstName) {
       this.firstName = firstName;
       //当数据发生变化，通知对应的变量的观察者
       notifyPropertyChanged(BR.firstName);
   }
   public void setLastName(String lastName) {
       this.lastName = lastName;
       notifyPropertyChanged(BR.lastName);
   }
}
```

#### ObservableFields
如果你发现通过`ObservableObject`定义数据类有些麻烦，那么，`ObservableFields`，就是更好的选择。不单单有`ObservableFields`,DataBinding还对所有基本数据类型做出了封装，可以直接使用.

```java
public class User {
    public final ObservableField<String> firstName = new ObservableField<>();
    public final ObservableField<String> lastName = new ObservableField<>();

    public final ObservableBoolean isSuperMan = new ObservableBoolean();
    public final ObservableByte bookNum = new ObservableByte();
    public final ObservableShort bookFans = new ObservableShort();
    public final ObservableInt age = new ObservableInt();
    public final ObservableLong money = new ObservableLong();
    public final ObservableChar sex = new ObservableChar();
    public final ObservableFloat bankMoney = new ObservableFloat();
    public final ObservableDouble dkMoney = new ObservableDouble();

    public final ObservableArrayList<String> children = new ObservableArrayList<>();
    public final ObservableMap<String, Integer> map = new ObservableArrayMap();
}

```

注意点：  

1. 因为`Observable对应的类型`，是对基本数据的包装，所以可以直接将变量定义为`final`