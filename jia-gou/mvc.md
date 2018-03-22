### 什么是MVC开发模式？
将程序分层，分别为Model层(负责数据管理，获取)，View层(负责交互界面)，Controller层(负责逻辑处理)，移动端与服务器端不同，此处不多说。

### 架构原理图(别人画法)
![](/assets/MVC别人画法.png)

### 架构原理图(我的画法)
![](/assets/MVC基础结构.png)

#### 说明
个人认为，以前并不会把`Controller`单独建立类，直接将`Fragment`与`Activity`作为`Controller`，所以从`Model`回来，到底是到`View上`还是`Controller`上？

我以为，从`Model`回来，最终是`Controller`的逻辑结束过程，由此引发`View`的改变则是`Controller`通知`View`变化.