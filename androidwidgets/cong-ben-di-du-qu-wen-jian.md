### WebView读取本地文件前提条件
当加载的网页中存在 `<input type="file">` 时，点击按钮需要读取本地文件。

![](/assets/html选取文件.png)

### 原生中存在的问题
1. 5.0 以前不存选择文件的公开API,只存在`@hide`的api
2. 5.0 以后可已在WebChromeClient中重写`onShowFileChooser`
3. 4.4 中不存在任何关于选择文件的api，连`@hide`的也没有

### 解决方案:
#### 1.在WebChromeClient中添加对应方法： 

```java
// For Android < 3.0
public void openFileChooser(ValueCallback<Uri> valueCallback) {
    uploadMessage = valueCallback;
    openImageChooserActivity();
}

// For Android  >= 3.0
public void openFileChooser(ValueCallback valueCallback, String acceptType) {
    uploadMessage = valueCallback;
    openImageChooserActivity();
}

//For Android  >= 4.1
public void openFileChooser(ValueCallback<Uri> valueCallback, String acceptType, String capture) {
    uploadMessage = valueCallback;
    openImageChooserActivity();
}

// For Android >= 5.0
@Override
public boolean onShowFileChooser(WebView webView, ValueCallback<Uri[]> filePathCallback, WebChromeClient.FileChooserParams fileChooserParams) {
    uploadMessageAboveL = filePathCallback;
    openImageChooserActivity();
    return true;
}
```

##### 如果有混淆处理，需要取消对`openFileChooser`的混淆
因为这个方法是`@hide`的，所以编译的SDK中没有，如果不做混淆处理，会将此方法混淆。

```
-keepclassmembers class * extends android.webkit.WebChromeClient{
        public void openFileChooser(...);
       }
```


#### 2.在Activity中调起本地选择器

```java
private void openImageChooserActivity() {
    Intent i = new Intent(Intent.ACTION_GET_CONTENT);
    i.addCategory(Intent.CATEGORY_OPENABLE);
    i.setType("image/*");
    if (i.resolveActivity(getPackageManager()) != null) {
        startActivityForResult(Intent.createChooser(i, "Image Chooser"), FILE_CHOOSER_RESULT_CODE);
    }
}
```