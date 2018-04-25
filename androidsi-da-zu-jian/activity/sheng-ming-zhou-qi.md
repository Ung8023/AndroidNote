## 生命周期方法
| 方法 |  释 |
| :---: | :---: |
| `onCreate` | 表示`Activity`正在被创建(可以做一些初始化工作，加载界面布局资源，初始化需要的数据) |
| `onReStart` | 表示`Activity`正在被重新启动。当`Activity`从不可见重新变为可见状态时，会被调用 |
| `onStart` | 表示`Activity`正在被启动，即将开始，这是Activity已经可见，但是还没有出现在前台，还无法和用户交互 |
| `onResume` | 表示`Activity`已经可见，并且出现在前台并开始活动。 |
| `onPause` | 表示`Activity`正在停止，正常情况下紧接着`onStop`会被执行，但是如果这时快速回到当前`Activity`,那么`onResume`会被调用。(此时可以存储数据、停止动画，但是**不能太耗时**) |
| `onStop` | 表示`Activity`即将停止，可以做一些稍微重量级的回收工作，**不能太耗时** |
| `onDestroy` | 表示`Activity`即将被销毁，可以做一些回收工作和最终资源的释放 |

## 典型情况
用户参与时，`Activity`经历的声明周期

### 生命周期

![](/assets/Activity典型情况下的生命周期.png)

#### 具体流程
1. 一个`Activity`，第一次启动: `onCreate --> onStart --> onResume`
1. 用户打开新的Activity，或者切换到桌面时(目前显示的`Acticity`经历的声明周期): `onPause --> onStop`
    
    特殊情况： `如果新Activity采用了透明主题，那么当前Activity不会回调onStop`
    
1. 当用户再次回到原`Activity`,回调如下: `onRestart --> onStart --> onResume`
1. 当用户按`back`键回退时: `onPause --> onStop --> onDestory`
1. `onPause`执行之后新`Activity`的`onCreate -> onStart -> onResume`才会执行，所以不能在`onPause`中做重量级操作。

## 异常情况
1. `Activity`被系统回收或者由于设备的`Configuration`发生改变从而导致`Activity`被销毁重建。
2. 资源不足导致低优先级的`Activity`被回杀死
    
    1. 前台`Activity` —— 正在和用户交互的`Activity`，优先级最高
    2. 可见但非前台`Activity` —— `Activity`中弹出了一个对话框，导致`Activity`可见，但是位于后台无法和用户交互
    3. 后台`Activity` —— 已经被暂停的`Activity`，比如执行了`onStop`，优先级最低。


### 生命周期

![](/assets/Activity异常情况下的声明周期.png)

