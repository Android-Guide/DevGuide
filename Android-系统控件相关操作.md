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


## 系统剪贴板使用

为了使用剪贴板，需要通过调用getSystemService()方法来实例化ClipboardManager的对象。它的语法如下：

```
ClipboardManager sysClipboard = (ClipboardManager)getSystemService(CLIPBOARD_SERVICE);
```

需要做的下一件事就是通过调用ClipData类的数据方法的相应类型来实例化ClipData对象。如果文本数据在newPlainText方法被调用。必须将数据设置为剪贴板管理器对象的剪辑。

```
ClipData myClip;
String text = "hello world";
myClip = ClipData.newPlainText("text", text);
sysClipboard.setPrimaryClip(myClip);
```

ClipData对象可以采取这三种形式和下面的函数用于创建的那些形式。

![](http://shjborage-public.qiniudn.com/2017-01-25-14853243533914.jpg)

为了粘贴数据，先要通过调用getPrimaryClip()方法拿到剪辑。并从点击就可 ClipData.Item 对象的项目。从对象将得到数据。它的语法如下：

```
ClipData abc = myClipboard.getPrimaryClip();
ClipData.Item item = abc.getItemAt(0);
String text = item.getText().toString();
```

### Refs
[Android Clipboard(复制/剪贴板)](http://www.cnblogs.com/exmyth/p/4603496.html)
[Android 复制文本内容到系统剪贴板的最简单实践](http://www.cnblogs.com/chengyujia/p/5033217.html)

## 长按菜单实现

定义上下文菜单资源

在 `res/menu` 目录下创建 `main_copy.xml` :

```
<menu xmlns:android="http://schemas.android.com/apk/res/android">

    <item
        android:id="@+id/action_copy"
        android:showAsAction="ifRoom"
        android:title="@string/action_copy"/>

</menu>
```

创建上下文菜单:
```
    @Override
    public void onCreateContextMenu(ContextMenu menu, View v,
            ContextMenuInfo menuInfo) {
        super.onCreateContextMenu(menu, v, menuInfo);
        getMenuInflater().inflate(R.menu.ret_copy, menu);
    }
```

为上下文菜单登记视图，只有登记了的视图才能启动上下文菜单:

```
et_xianshi = (TextView) findViewById(R.id.tv_jisuan);
registerForContextMenu(et_xianshi);
```

响应上下文菜单

```
    @Override
	public boolean onContextItemSelected(MenuItem item) {
		// AdapterContextMenuInfo info =
		// (AdapterContextMenuInfo)item.getMenuInfo();

		switch (item.getItemId()) {
		case R.id.action_copy:
			String current = (String) et_xianshi.getText();
			current = current.replace(",", "");

			Log.e("shjborage", "you have click copy current value :" + current);

			return true;
		}

		return super.onContextItemSelected(item);
	}
```

### Refs
[Android上下文菜单，长按出现的菜单](http://blog.csdn.net/c15522627353/article/details/47974571)
