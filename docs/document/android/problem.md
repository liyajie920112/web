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

## 3. 
