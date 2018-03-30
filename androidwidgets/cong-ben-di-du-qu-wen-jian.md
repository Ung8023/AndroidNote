### WebView读取本地文件前提条件
当加载的网页中存在 `<input type="file">` 时，点击按钮需要读取本地文件。

![](/assets/html选取文件.png)

### 原生中存在的问题
1. 5.0 以前不存选择文件的公开API,只存在`@hide`的api
2. 5.0 以后可已在ChromeClient中重写`onShowFileChooser`
3. 4.4 中不存在任何关于选择文件的api，连`@hide`的也没有

### 解决方案:
在WebChromeClient