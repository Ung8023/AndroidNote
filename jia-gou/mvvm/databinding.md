### DataBinding 介绍
#### 什么是DataBinding
Google推出的既灵活又兼容低版本的支持库，使编码更简单，减少一些无意义的代码
#### 为什么使用 DataBinding？
1. 能减少无意义的代码，如`findViewById()`,`setEnabled()`...
2. 可以用来构建MVVM，当然，其他的任何开发模式也可以使用它
3. 是一个支持库，最低兼容的到Android 2.1

#### 使用前提
1. 目标**Android** 版本在***2.1***以上
2. **gradle** 插件版本在 ***1.5.0-alpha*** 或以上
3. 如果使用`DataBindingCompiler V2`,**gradle**版本需要在***3.1.0 Canary 6***或以上

#### 使用DataBinding
