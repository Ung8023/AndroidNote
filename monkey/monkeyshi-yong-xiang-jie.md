monkey可以运行在模拟器或实际设备中，它向系统发送伪随机的用户事件流，实现对正在开发的应用程序进行压力测试，是一种为了**测试软件稳定性、健壮性**的快速有效方法。

## 基本使用

1. 进入adb shell
2. 运行`system/bin`路径下monkey脚本

```
$ adb shell
# cd /system/bin
# monkey

或
$ adb shell /system/bin/monkey


## 添加<event-count>,事件数
$ adb shell monkey <event-count>

```



