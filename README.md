## 01 公司项目域名地址不正确

### 静态变量或常量引发的问题
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
