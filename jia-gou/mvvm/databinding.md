### DataBinding 介绍

#### 什么是DataBinding

Google推出的既灵活又兼容低版本的支持库，使编码更简单，减少一些无意义的代码

#### 为什么使用 DataBinding？

1. 能减少无意义的代码，如`findViewById()`,`setEnabled()`...
2. 可以用来构建MVVM，当然，其他的任何开发模式也可以使用它
3. 是一个支持库，最低兼容的到Android 2.1

#### 使用前提

1. 目标**Android** 版本在_**2.1**_以上
2. **gradle** 插件版本在 _**1.5.0-alpha**_ 或以上
3. 如果使用`DataBindingCompiler V2`,**gradle**版本需要在_**3.1.0 Canary 6**_或以上

#### 使用DataBinding
在Moudel级别的`build.gradle`中添加:  

```java
android{
    ...
    dataBinding {
    enabled = true
    }
}
```

##### 使用DataBindingCompiler V2
在`grable.properties`中添加  

```java
 android.databinding.enableV2=true
```

## 经过以上配置，你就可以开心的使用DataBinding了

### 第一个Demo
#### Layout文件(`activity_demo.xml`)
```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android">
   <data>
       <variable name="user" type="com.example.User"/>
   </data>
   <LinearLayout
       android:orientation="vertical"
       android:layout_width="match_parent"
       android:layout_height="match_parent">
       <TextView android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="@{user.firstName}"/>
       <TextView android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="@{user.lastName}"/>
   </LinearLayout>
</layout>
```

#### `User.java`文件
```java
public class User {
   private final String firstName;
   private final String lastName;
   public User(String firstName, String lastName) {
       this.firstName = firstName;
       this.lastName = lastName;
   }
   public String getFirstName() {
       return this.firstName;
   }
   public String getLastName() {
       return this.lastName;
   }
}
```

#### `ActivityDemo.java`
```
@Override
protected void onCreate(Bundle savedInstanceState) {
   super.onCreate(savedInstanceState);
   MainActivityBinding binding = DataBindingUtil.setContentView(this, R.layout.main_activity);
   User user = new User("Test", "User");
   binding.setUser(user);
}
```

### DataBinding工作过程
#### 前提
已经配置好了，DataBinding环境

#### 工作原理图
![](/assets/DataBinding工作原理.png)

