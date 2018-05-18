### 四种启动模式
| 启动模式 | 释 |
| :---: | :--- |
| `standard` | `默认模式`,每次启动一个`Activity`都会创建新的实例，一个任务栈中可以有多个此`Activity`实例，当启动`standard`模式的`Activity`时，被启动的`Activity`会进入启动它的那个`Activity`所在的任务栈，所以这个启动模式的`Activity`不能直接用`ApplicationContext`启动(`ApplicationContext`没有任务栈)。 |
| `singleTop` | 栈顶复用模式，如果此`Activity`已经在栈中有一个实例，并且处于栈顶，那么再次启动此`Activity`，`Activity`不会被重建，同时它的`onNewIntent`方法会被回调。 如果`Activity`不在栈顶，则会重新创建|
| `singleTask` | 栈内复用模式，只要栈中有这个`Activity`,那么就不会重新创建，只会回调`onNewIntent` |