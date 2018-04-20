## 声明周期方法
| 方法 |  释 |
| :---: | :---: |
| `onCreate` | 表示Activity正在被创建(可以做一些初始化工作，加载界面布局资源，初始化需要的数据) |
| `onReStart` | 表示Activity正在被重新启动。

## 典型情况
用户参与时，`Activity`经历的声明周期

### 声明周期



## 异常情况
`Activity`被系统回收或者由于设备的`Configuration`发生改变从而导致`Activity`被销毁重建。
