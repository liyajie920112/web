# 在维护Android项目的时候遇到的一些问题

## 1. 当我们修改系统的字体大小的时候, webview中的字体会跟着变化, 如果我们不想要webview中的字体大小跟着android手机设置的字体大小变化, 
我们可以给webview设置:`webSettings.setTextZoom(100); `
如果我们在原生的组件中设置字体的时候用的是sp, 则也会跟随这系统字体大小进行变化, 如果不想让字体变化则需要使用dp

## 2. Android中 Bottom Sheet的实现

第一步

`
// 版本必须大于等于23
compile 'com.android.support:appcompat-v7:23.2.1'
compile 'com.android.support:design:23.2.1'
`
  
第二步 
```java
BottomSheetDialog mBottomSheetDialog = new BottomSheetDialog(getActivity());
// R.layout.act_share_dialog 是我们要显示在Bottom Sheet中的内容布局
View view = getActivity().getLayoutInflater().inflate(R.layout.act_share_dialog, null);
findViewByIdShare(view);
mBottomSheetDialog.setContentView(view);
mBottomSheetDialog.show(); // 显示
```
好处, 主要是方便, 不用我们自己去处理隐藏事件, 默认会有遮罩, 并且也可以我们自定义, 具体我没有实践

[BottomSheetDialog参考地址](https://github.com/itdais/MaterialDesignDing)

## 3. 检查通知权限是否打开了

```java
// 返回true说明开启了, false: 没开启
public Boolean checkNotifyEnabled(Context context) {
		String CHECK_OP_NO_THROW = "checkOpNoThrow";
		String OP_POST_NOTIFICATION = "OP_POST_NOTIFICATION";
		AppOpsManager mAppOps = (AppOpsManager)
				context.getSystemService(Context.APP_OPS_SERVICE);
		ApplicationInfo appInfo = context.getApplicationInfo();
		String pkg = context.getApplicationContext().getPackageName();
		int uid = appInfo.uid;
		Class appOpsClass = null; /* Context.APP_OPS_MANAGER */
		try {
			appOpsClass = Class.forName(AppOpsManager.class.getName());
			Method checkOpNoThrowMethod = appOpsClass.getMethod(CHECK_OP_NO_THROW, Integer.TYPE, Integer.TYPE, String.class);
			Field opPostNotificationValue = appOpsClass.getDeclaredField(OP_POST_NOTIFICATION);
			int value = (int) opPostNotificationValue.get(Integer.class);
			return ((int) checkOpNoThrowMethod.invoke(mAppOps, value, uid, pkg) == AppOpsManager.MODE_ALLOWED);
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		} catch (NoSuchMethodException e) {
			e.printStackTrace();
		} catch (NoSuchFieldException e) {
			e.printStackTrace();
		} catch (InvocationTargetException e) {
			e.printStackTrace();
		} catch (IllegalAccessException e) {
			e.printStackTrace();
		}
		return false;
}
```
如果没开启则跳转到改应用的开启页面
```java
public void goToSetting() {
    Intent localIntent = new Intent();
    localIntent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
    if (Build.VERSION.SDK_INT >= 9) {
        localIntent.setAction("android.settings.APPLICATION_DETAILS_SETTINGS");
        localIntent.setData(Uri.fromParts("package", getPackageName(), null));
    } else if (Build.VERSION.SDK_INT <= 8) {
        localIntent.setAction(Intent.ACTION_VIEW);
        localIntent.setClassName("com.android.settings", "com.android.setting.InstalledAppDetails");
        localIntent.putExtra("com.android.settings.ApplicationPkgName", getPackageName());
    }
    startActivity(localIntent);
}
```

持续更新...
