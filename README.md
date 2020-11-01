## 01 静态变量或常量引发的问题
当有如下代码时，一定要注意这样的HOST只会被赋值一次，而后就算怎么改变QkAppProps中的HOST值都不会对其造成影响。

```
public class QSConstants {
    public static final String HOST = QkAppProps.getHost();
}
```
因为HOST为静态常量，所以当类被加载时，就会把QkAppProps.getHost()的值赋值给HOST。

所以容易引发的问题就是：HOST被赋值时，QkAppProps.getHost()还未被初始化为正确的值。

单例类也应该注意这个问题。

具体到代码中：feed流修改了一个版本号，导致`AbilityFromHostForVideoFeed`类中的

```
@Override
public String getFeedServerAddress() {
    return QSConstants.getFeedServerAddress();
}
```
方法被提前调用，从而导致QSConstants被初始化，导致HOST被提前赋值。

这个问题很容易被忽视而导致怎么排查都百思不得解，一切还是基础

## 02 状态栏
隐藏状态栏，如果是`AppCompatActivity`，则使用
```
if (getSupportActionBar() != null) {
    getSupportActionBar().hide();
}
```
如果继承Activity，则使用
```
requestWindowFeature(Window.FEATURE_NO_TITLE)
```

## 03 dialog
### dialog设置setContentView不显示自己设置的布局
```
setContentView(R.layout.loading_alert);
```
在base lib中设置公共的loading view，结果发现使用R.layout.loading_alert为布局时，不显示自己在R.layout.loading_alert文件中设置的布局，而是系统自带的一个布局，后来怀疑是loading_alert这个名字在系统中也定义过，这里优先找到了系统中的这个布局文件。





