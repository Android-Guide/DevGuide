# Android 系统控件相关操作

## 系统导航栏
Android 默认标题栏比较不好控制，一般要去掉自定义，自定义的方法常见的如下：

在AndroidManifest.xml文件中定义（整个应用）:

```
<application android:icon="@drawable/icon"
android:label="@string/app_name"
android:theme="@android:style/Theme.NoTitleBar">
```

### Refs
[Android 取消标题栏方法汇总](http://blog.csdn.net/zjt107/article/details/43237319)  
[Android开发:去掉Activity的头部标题栏及全屏显示](http://blog.csdn.net/jonahzheng/article/details/8776552/)  
[android应用中去掉标题栏的方法](http://blog.csdn.net/liuzhidong123/article/details/7818531)  
