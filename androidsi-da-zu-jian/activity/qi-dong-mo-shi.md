### 四种启动模式
| 启动模式 | 释 |
| :---: | :--- |
| `standard` | `默认模式`,每次启动一个`Activity`都会创建新的实例，一个任务栈中可以有多个此`Activity`实例，当启动`standard`模式的`Activity`时，被启动的`Activity`会进入启动它的那个`Activity`所在的任务栈，所以这个启动模式的`Activity`不能直接用`ApplicationContext`启动。 |