### 说明
ViewModel用来存储和管理UI需要的数据。Google提供了一个ViewModel的实现，如果不喜欢，可以自己写。这里只介绍官方ViewModel。

### 使用ViewModel
在Module级别的build.gradle中添加：

```java
dependencies {
    implementation "android.arch.lifecycle:viewmodel:1.1.1"
}

```