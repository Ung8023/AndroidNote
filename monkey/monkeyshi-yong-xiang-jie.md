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

### 查看monkey脚本
批处理调用的是`com.android.commands.monkey.Monkey`包，如果希望定制monkey，则需要修改这个包。

```
base=/system
export CLASSPATH=$base/framework/monkey.jar
trap "" HUP
exec app_process $base/bin com.android.commands.monkey.Monkey $*
```

## monkey的命令及其使用
```
$ adb shell monkey [options] <event-count>
```

### 常规命令
#### 查看帮助
```
$ adb shell monkey -h
```

#### 打印日志信息
只需要在monkey命令下加`-v`参数即可打印日志信息，可以添加多个`-v`，`-v`越多日志信息越详细，目前最多支持3个-v

```
$ adb shell monkey -v -v -v 1000
```

1. 0级 除启动提示，测试完成和最终结果提供较少信息
2. 1级 提供较详细测试信息
3. 2级 提供更详细的安装信息

### 事件类命令
