### WebView读取本地文件前提条件
当加载的网页中存在 `<input type="file">` 时，点击按钮需要读取本地文件。

![](/assets/html选取文件.png)

### 原生中存在的问题
1. 5.0 以前不存选择文件的公开API
2. 5.0 以后可已在ChromeClient中重写`onShowFileChooser`