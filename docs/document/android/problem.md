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

## 4. android使用远程图片

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

## 5. Android使用远程图片优化

```java
package com.hbyc.horseinfo.activity.pay;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.Collections;
import java.util.Map;
import java.util.WeakHashMap;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

import android.annotation.SuppressLint;
import android.app.Activity;
import android.content.Context;
import android.graphics.Bitmap;
import android.graphics.Bitmap.Config;
import android.graphics.BitmapFactory;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.graphics.PorterDuff.Mode;
import android.graphics.PorterDuffXfermode;
import android.graphics.Rect;
import android.graphics.RectF;
import android.os.Handler;
import android.os.Message;
import android.widget.ImageView;
@SuppressLint("HandlerLeak")
@SuppressWarnings("unused")
public class ImageLoader {

	// LinkedHashMap集合
	MemoryCache memoryCache = new MemoryCache();
	// 一个线程池
	ExecutorService executorService;
	// 文件缓存
	FileCache fileCache;
	// 弱引用的集合
	private Map<ImageView, String> imageViews = Collections
			.synchronizedMap(new WeakHashMap<ImageView, String>());
	private int stub_id;
	private static Context mContext;

	// 单例模式
	private static ImageLoader loader;
	private Bitmap bitmap;

	// 回调接口
	public interface ImageCallBack {
		public void loadImage(Bitmap bitmap);
	}

	public static ImageLoader getInstance(Context context) {
		mContext = context;
		if (loader == null) {
			loader = new ImageLoader(context);
		}
		return loader;
	}

	// 私有的构造方法
	private ImageLoader(Context context) {
		fileCache = new FileCache(context);
		executorService = Executors.newFixedThreadPool(5);
	}

	// 显示图片
	public void displayImage(String url, ImageView imageView) {
		// 先将地址和imageView的键值对放入到弱引用的集合中
		imageViews.put(imageView, url);
		// 从内存缓存中拿缓存
		bitmap = memoryCache.get(url);
		if (bitmap != null) {
			imageView.setImageBitmap(bitmap);
		} else {
			// 如果缓存不存在
			queuePhoto(url, imageView);
			imageView.setImageResource(getStub_id());
		}
	}

	// 生成圆角图片
	public static Bitmap GetRoundedCornerBitmap(Bitmap bitmap) {
		try {
			Bitmap output = Bitmap.createBitmap(bitmap.getWidth(),
					bitmap.getHeight(), Config.ARGB_8888);
			Canvas canvas = new Canvas(output);
			final Paint paint = new Paint();
			final Rect rect = new Rect(0, 0, bitmap.getWidth(),
					bitmap.getHeight());
			final RectF rectF = new RectF(new Rect(0, 0, bitmap.getWidth(),
					bitmap.getHeight()));
			final float roundPx = 10;
			paint.setAntiAlias(true);
			canvas.drawARGB(0, 0, 0, 0);
			paint.setColor(Color.BLACK);
			canvas.drawRoundRect(rectF, roundPx, roundPx, paint);
			paint.setXfermode(new PorterDuffXfermode(Mode.SRC_IN));
			final Rect src = new Rect(0, 0, bitmap.getWidth(),
					bitmap.getHeight());
			canvas.drawBitmap(bitmap, src, rect, paint);
			return output;
		} catch (Exception e) {
			return bitmap;
		}
	}

	public void displayImage(String url, ImageView imageView, int stud_id) {
		setStub_id(stud_id);
		displayImage(url, imageView);
	}

	//
	private void queuePhoto(String url, ImageView imageView) {
		PhotoToLoad p = new PhotoToLoad(url, imageView);
		executorService.submit(new PhotosLoader(p));
	}

	// 联网下载图片的方法, 被 PhotosLoader 线程对象调用
	public Bitmap getBitmap(String url) {
		// 从文件缓存中拿文件
		File f = fileCache.getFile(url);
		// Bitmap b = decodeFile(f);
		Bitmap b = compressImageFromFile(f);
		if (b != null) {
			return b;
		}
		// 文件缓存中不存在,就联网获得文件
		try {
			Bitmap bitmap = null;
			URL imageUrl = new URL(url);
			HttpURLConnection conn = (HttpURLConnection) imageUrl
					.openConnection();
			conn.setConnectTimeout(30000);
			conn.setReadTimeout(30000);
			conn.setInstanceFollowRedirects(true);
			InputStream is = conn.getInputStream();
			OutputStream os = new FileOutputStream(f);
			CopyStream(is, os);
			os.close();
			// bitmap = decodeFile(f);
			bitmap = compressImageFromFile(f);
			return bitmap;
		} catch (Exception ex) {
			//ex.printStackTrace();
			return null;
		}
	}

	// 压缩图片
	private Bitmap compressImageFromFile(File f) {
		BitmapFactory.Options newOpts = new BitmapFactory.Options();
		newOpts.inJustDecodeBounds = true;// 只读边,不读内容
		Bitmap bitmap;
		try {

			if (!f.exists()) {
				return null;
			} else {
				bitmap = BitmapFactory.decodeStream(new FileInputStream(f),
						null, newOpts);

				newOpts.inJustDecodeBounds = false;
				int w = newOpts.outWidth;
				int h = newOpts.outHeight;
				float hh = 800f;//
				float ww = 480f;//
				int be = 1;
				if (w > h && w > ww) {
					be = (int) (newOpts.outWidth / ww);
				} else if (w < h && h > hh) {
					be = (int) (newOpts.outHeight / hh);
				}
				if (be <= 0)
					be = 1;
				newOpts.inSampleSize = be;// 设置采样率

				newOpts.inPreferredConfig = Config.ARGB_8888;// 该模式是默认的,可不设
				newOpts.inPurgeable = true;// 同时设置才会有效
				newOpts.inInputShareable = true;// 。当系统内存不够时候图片自动被回收

				bitmap = BitmapFactory.decodeStream(new FileInputStream(f),
						null, newOpts);

				// return compressBmpFromBmp(bitmap);//原来的方法调用了这个方法企图进行二次压缩
				// 其实是无效的,大家尽管尝试
				return bitmap;

			}
		} catch (FileNotFoundException e1) {
			// TODO Auto-generated catch block
			e1.printStackTrace();
		}
		return null;

	}

	public Bitmap decodeFile(File f) {
		try {
			BitmapFactory.Options options = new BitmapFactory.Options();
			options.inJustDecodeBounds = true;
			BitmapFactory.decodeStream(new FileInputStream(f), null, options);

			options.inSampleSize = 1;
			int outWidth = options.outWidth;
			int outHeight = options.outHeight;
			options.inJustDecodeBounds = false;
			options.inPreferredConfig = Bitmap.Config.RGB_565;
			options.inPurgeable = true;
			options.inInputShareable = true;
			return BitmapFactory.decodeStream(new FileInputStream(f), null,
					options);
		} catch (FileNotFoundException e) {
		}
		return null;
	}

	/**
	 * 下载图片的线程对象
	 * 
	 * @author JinnyZh
	 */
	class PhotosLoader implements Runnable {
		PhotoToLoad photoToLoad;

		PhotosLoader(PhotoToLoad photoToLoad) {
			this.photoToLoad = photoToLoad;
		}

		@Override
		public void run() {
			// 如果这个地址和imageView的键值对在软引用的集合中不存在,直接返回
			if (imageViewReused(photoToLoad)) {
				return;
			}
			Bitmap bmp = getBitmap(photoToLoad.url);
			if (bmp != null) {
//				bmp = GetRoundedCornerBitmap(bmp);
			}
			memoryCache.put(photoToLoad.url, bmp);
			if (imageViewReused(photoToLoad))
				return;
			BitmapDisplayer bd = new BitmapDisplayer(bmp, photoToLoad);
			Activity a = (Activity) photoToLoad.imageView.getContext();
			a.runOnUiThread(bd);
		}
	}

	public void getBitmapAsync(final String url, final ImageCallBack callback) {
		final Handler handler = new Handler() {
			public void handleMessage(Message msg) {
				if (msg.obj != null) {
					Bitmap bitmap = (Bitmap) msg.obj;
					callback.loadImage(bitmap);
				}
			};
		};

		new Thread() {
			public void run() {
				Message msg = Message.obtain();
				msg.obj = getBitmap(url);
				handler.sendMessage(msg);
			};
		}.start();
	}

	/**
	 * 查看软引用集合中是否存在该 imageView 和 url 的键值对,
	 * 
	 * @param photoToLoad
	 * @return 没有,就返回true
	 */
	boolean imageViewReused(PhotoToLoad photoToLoad) {
		String tag = imageViews.get(photoToLoad.imageView);
		if (tag == null || !tag.equals(photoToLoad.url)) {
			return true;
		}
		return false;
	}

	class BitmapDisplayer implements Runnable {
		Bitmap bitmap;
		PhotoToLoad photoToLoad;

		public BitmapDisplayer(Bitmap b, PhotoToLoad p) {
			bitmap = b;
			photoToLoad = p;
		}

		public void run() {
			if (imageViewReused(photoToLoad))
				return;
			if (bitmap != null)
				photoToLoad.imageView.setImageBitmap(bitmap);
			else
				photoToLoad.imageView.setImageResource(getStub_id());
		}
	}

	public void clearCache() {
		memoryCache.clear();
		fileCache.clear();
	}

	public void clearMemoryCache() {
		memoryCache.clear();
	}

	public static void CopyStream(InputStream is, OutputStream os) {
		final int buffer_size = 1024;
		try {
			byte[] bytes = new byte[buffer_size];
			for (;;) {
				int count = is.read(bytes, 0, buffer_size);
				if (count == -1)
					break;
				os.write(bytes, 0, count);
			}
		} catch (Exception ex) {
		}
	}

	public void setStub_id(int stub_id) {
		this.stub_id = stub_id;
	}

	public int getStub_id() {
		return stub_id;
	}
}
```

## 6. Android打开相机和相册

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

## 7. Android图片压缩

```java
public static Bitmap comp(Bitmap image) {

	ByteArrayOutputStream baos = new ByteArrayOutputStream();
	image.compress(Bitmap.CompressFormat.JPEG, 100, baos);
	if( baos.toByteArray().length / 1024>1024) {//判断如果图片大于1M,进行压缩避免在生成图片（BitmapFactory.decodeStream）时溢出
		baos.reset();//重置baos即清空baos
		image.compress(Bitmap.CompressFormat.JPEG, 50, baos);//这里压缩50%，把压缩后的数据存放到baos中
	}
	ByteArrayInputStream isBm = new ByteArrayInputStream(baos.toByteArray());
	BitmapFactory.Options newOpts = new BitmapFactory.Options();
	//开始读入图片，此时把options.inJustDecodeBounds 设回true了
	newOpts.inJustDecodeBounds = true;
	Bitmap bitmap = BitmapFactory.decodeStream(isBm, null, newOpts);
	newOpts.inJustDecodeBounds = false;
	int w = newOpts.outWidth;
	int h = newOpts.outHeight;
	//现在主流手机比较多是800*480分辨率，所以高和宽我们设置为
	float hh = 800f;//这里设置高度为800f
	float ww = 480f;//这里设置宽度为480f
	//缩放比。由于是固定比例缩放，只用高或者宽其中一个数据进行计算即可
	int be = 1;//be=1表示不缩放
	if (w > h && w > ww) {//如果宽度大的话根据宽度固定大小缩放
		be = (int) (newOpts.outWidth / ww);
	} else if (w < h && h > hh) {//如果高度高的话根据宽度固定大小缩放
		be = (int) (newOpts.outHeight / hh);
	}
	if (be <= 0)
		be = 1;
	newOpts.inSampleSize = be;//设置缩放比例
	//重新读入图片，注意此时已经把options.inJustDecodeBounds 设回false了
	isBm = new ByteArrayInputStream(baos.toByteArray());
	bitmap = BitmapFactory.decodeStream(isBm, null, newOpts);
	return compressImage(bitmap);//压缩好比例大小后再进行质量压缩
}

// 质量压缩, 分辨率并不会变化
private static Bitmap compressImage(Bitmap image) {

	ByteArrayOutputStream baos = new ByteArrayOutputStream();
	image.compress(Bitmap.CompressFormat.JPEG, 100, baos);//质量压缩方法，这里100表示不压缩，把压缩后的数据存放到baos中
	int options = 100;
	while ( baos.toByteArray().length / 1024>100) { //循环判断如果压缩后图片是否大于100kb,大于继续压缩
		baos.reset();//重置baos即清空baos
		image.compress(Bitmap.CompressFormat.JPEG, options, baos);//这里压缩options%，把压缩后的数据存放到baos中
		options -= 10;//每次都减少10
	}
	ByteArrayInputStream isBm = new ByteArrayInputStream(baos.toByteArray());//把压缩后的数据baos存放到ByteArrayInputStream中
	Bitmap bitmap = BitmapFactory.decodeStream(isBm, null, null);//把ByteArrayInputStream数据生成图片
	return bitmap;
}
```

## 8. Android中创建Dialog

```java
private void showDialog(String content) {
	final Dialog alertDialog = new Dialog(this, R.style.dialog1);
	View view = LayoutInflater.from(this).inflate(
			R.layout.order_alertdialog, null);
	TextView alert_content = (TextView) view
			.findViewById(R.id.alert_content);
	Button bt_sure = (Button) view.findViewById(R.id.bt_sure);
	alert_content.setText(content);
	bt_sure.setOnClickListener(new OnClickListener() {
		@Override
		public void onClick(View v) {
			alertDialog.dismiss();
		}
	});
	alertDialog.setCanceledOnTouchOutside(false);
	alertDialog.setContentView(view);
	alertDialog.show();
}
```

R.style.dialog1内容如下(src/main/res/values/styles.xml)

```xml
<style name="dialog1" parent="@android:style/Theme.Dialog">
	<item name="android:windowFrame">@null</item>
	<item name="android:windowIsFloating">true</item>
	<item name="android:windowIsTranslucent">false</item>
	<item name="android:windowNoTitle">true</item>
	<item name="android:background">@android:color/transparent</item>
	<item name="android:backgroundDimEnabled">true</item>
	<item name="android:windowBackground">@android:color/transparent</item>
</style>
```

## 9. Android去掉Dialog默认内边距

```java
Window window = mDialog.getWindow();
window.getDecorView().setPadding(0, 0, 0, 0);
window.setLayout(LayoutParams.MATCH_PARENT, LayoutParams.MATCH_PARENT);
```

## 10. Android定时器Timer使用

```java
Timer timer = new Timer();
Handler handler = new Handler(Looper.getMainLooper());
TimerTask task = new TimerTask() {
    @Override
    public void run() {
	handler.post(new Runnable() { // 要在主线程中执行
	    @Override
	    public void run() {
		// 发起请求, 检查支付状态
		RequestUtil.checkoutPayStatus(context, out_trade_no, new JsonGetCallback() {

		    @Override
		    public void getNetString(String str) {
			String status = JsonUtil.getStrValue(str, "status");
			if (status.equals("1")) {
			    dialog.dismiss();
			    Activity a = (Activity) context;
			    a.finish();
			    ToastUtil.shortToast(context, "支付成功");
			}
		    }
		});
	    }
	});

    }
};
timer.schedule(task, 10 * 1000, 5 * 1000); // 十秒后执行第一次, 之后每隔5s执行一次
```

持续更新...
