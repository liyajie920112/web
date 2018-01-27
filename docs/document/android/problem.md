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

## android使用远程图片

```java
private Handler handler = new Handler(); // 用于通知主线程修改ui

private void initImage(final ImageView iv, final String imgurl) {
	// 需要在子线程中加载远程图片
	final Bitmap[] bitmap = {null};

	final Runnable changeBanner = new Runnable() {
	    @Override
	    public void run() {
		if (bitmap[0] != null) {
		    iv.setImageBitmap(bitmap[0]);
		}
	    }
	};

	new Thread() {
	    public void run() {
		try {
		    bitmap[0] = CommonUtils.getHttpBitmap(imgurl);
		    handler.post(changeBanner); // 需要在主线程中更新ui
		} catch (Exception e) {
		    e.printStackTrace();
		}
	    }
	}.start();

}
```

CommonUtils.getHttpBitmap() // 方法如下

```java
// url: 远程图片url, 如: http://2t.5068.com/uploads/allimg/120410/1140006131-3.jpg
public static Bitmap getHttpBitmap(String url){
	URL myFileURL;
	Bitmap bitmap=null;
	try{
		myFileURL = new URL(url);
		//获得连接
		HttpURLConnection conn=(HttpURLConnection)myFileURL.openConnection();
		//设置超时时间为6000毫秒，conn.setConnectionTiem(0);表示没有时间限制
		conn.setConnectTimeout(6000);
		//连接设置获得数据流
		conn.setDoInput(true);
		//不使用缓存
		conn.setUseCaches(false);
		//这句可有可无，没有影响
		//conn.connect();
		//得到数据流
		InputStream is = conn.getInputStream();
		//解析得到图片
		bitmap = BitmapFactory.decodeStream(is);
		//关闭数据流
		is.close();
	}catch(Exception e){
		e.printStackTrace();
	}
	return bitmap;
}
```

## Android打开相机和相册

```java
private final int REQUEST_TAKE_PHOTO = 1010; // 自定义即可
private final int REQUEST_CHOOSE_PHOTO = 1011; // 自定义即可, 主要用户在用户拍完照或者选择图片之后回到当前Activity中指定onActivityResult方法的时候进行判断是拍照后过来的还是在相册中过来的
// 打开相机
Intent takePictureIntent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
startActivityForResult(takePictureIntent, REQUEST_TAKE_PHOTO);//跳转界面传回拍照所得数据

// 打开相册
Intent i = new Intent(Intent.ACTION_PICK, MediaStore.Images.Media.EXTERNAL_CONTENT_URI);
startActivityForResult(i, REQUEST_CHOOSE_PHOTO);
```
### 将选择好或者拍的照片展示到`ImageView`中
```java
// `data` -> `onActivityResult`方法形参中的 `Intent data`
private void showYourPic(Intent data) {
	Uri uri = data.getData();

	Bitmap bitmap = null;
	if (uri != null) {
	    try {
		bitmap = MediaStore.Images.Media.getBitmap(getContentResolver(), uri);
	    } catch (Exception e) {
		e.printStackTrace();
	    }
	} else {
	    Bundle bundle = data.getExtras(); // 有的型号的手机数据可能在Extra中
	    bitmap = (Bitmap) bundle.get("data");
	}

	// 这里设置选择的图片
	ByteArrayOutputStream baos = null;
	try {
	    baos = new ByteArrayOutputStream();
	    bitmap.compress(Bitmap.CompressFormat.JPEG, 70, baos); // 进行图片压缩, 100->则不压缩
	} finally {
	    try {
		if (baos != null) {
		    baos.close();
		}
	    } catch (Exception ee) {
		ee.printStackTrace();
	    }
	}
	ivImg.setImageBitmap(bitmap); // ivImg->获取到的ImageView
}
```

持续更新...
