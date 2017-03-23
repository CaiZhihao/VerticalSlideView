# VerticalSlideView

#### 在原库上作了更改优化。

###
类似淘宝的商品详情页，继续拖动查看详情，其中拖动增加了阻尼，并且重写了ListView，GridView，ScrollView，WebView，RecyclerView 的 dispatchTouchEvent 方法，使用的时候无须额外的代码，可以任意嵌套使用。

该项目参考了：[https://github.com/xmuSistone/android-vertical-slide-view](https://github.com/xmuSistone/android-vertical-slide-view) 喜欢原作的可以去使用。相比原项目，代码更简单易懂，扩展性更高，欢迎大家下载体验本项目，如果使用过程中遇到什么问题，欢迎反馈。

### 联系方式
 * 邮箱地址： liaojeason@126.com
 * QQ群： 489873144 （建议使用QQ群，邮箱使用较少，可能看的不及时）
 * 本群刚建立，旨在为使用我的github项目的人提供方便，如果遇到问题欢迎在群里提问。个人能力也有限，希望一起学习一起进步。


## 演示
 ![image](http://7xss53.com2.z0.glb.clouddn.com/verticalslideview/demo0.png)![image](http://7xss53.com2.z0.glb.clouddn.com/verticalslideview/demo2.gif)![image](http://7xss53.com2.z0.glb.clouddn.com/verticalslideview/demo3.gif)![image](http://7xss53.com2.z0.glb.clouddn.com/verticalslideview/demo4.gif)

## 1.用法
该项目和我github上其他的view相关的项目已经一起打包上传到jCenter仓库中（源码地址 [https://github.com/jeasonlzy0216/ViewCore](https://github.com/jeasonlzy0216/ViewCore) ），使用的时候可以直接使用compile依赖，用法如下
```java
	compile 'com.lzy.widget:view-core:0.2.3'
```
或者使用
```java
    compile project(':verticalslide')
```

## 2.实现原理
把ListView，GridView，ScrollView，WebView，RecyclerView 的 dispatchTouchEvent 方法进行重写，当这几个View在对顶部并且向下拉 或者 在对底部向上拉时，自身不消费事件，让父容器拦截事件并处理。并支持ViewPager。

## 3.代码参考
### 1.对于加载下一页的监听，只需要初始化控件并且设置监听即可
```java
	DragSlideLayout dragLayout = (DragSlideLayout) findViewById(R.id.dragLayout);
	dragLayout.setOnShowNextPageListener(new DragSlideLayout.OnShowNextPageListener() {
        @Override
        public void onShowNextPage() {
            fragment_webView.initView();
        }
    });
```
### 2.支持快速返回第一页
```java
    @Override
    public void onClick(View v) {
        /**
         * 返回顶部分三步
         * 1.第二页滚动到第二页的顶部
         * 2.VerticalSlide从第二页返回第一页
         * 3.第一页滚动到第一页的顶部
         * OnGoTopListener 表示第一页滚动到顶部 的方法,这个由于采用什么布局,库内部并不知道,所以一般是自己实现
         * 也可以不实现,直接传null
         */
        bottomFragment.goTop();
        verticalSlide.goTop(new VerticalSlide.OnGoTopListener() {
            @Override
            public void goTop() {
                topFragment.goTop();
            }
        });
    }
```
### 3.使用`DragSlideLayout`控件，内部包含两个子View，分别表示第一页和第二页，可以先使用`FrameLayout`占位，代码中使用Fragment替换
```xml
	<?xml version="1.0" encoding="utf-8"?>
	<RelativeLayout
	    xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent">
	
	    <com.lzy.ui.DragSlideLayout
	        android:id="@+id/dragLayout"
	        android:layout_width="match_parent"
	        android:layout_height="match_parent">
	
	        <FrameLayout
	            android:id="@+id/first"
	            android:layout_width="match_parent"
	            android:layout_height="match_parent"/>
	
	        <FrameLayout
	            android:id="@+id/second"
	            android:layout_width="match_parent"
	            android:layout_height="match_parent"/>
	    </com.lzy.ui.DragSlideLayout>
	</RelativeLayout>
```
### 4.也可以直接使用`VerticalScrollView`,或者`VerticalListView`，等替换，上下两个布局可以任意调换
```xml
	<?xml version="1.0" encoding="utf-8"?>
	<RelativeLayout
	    xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent">
	
	    <com.lzy.ui.DragSlideLayout
	        android:id="@+id/dragLayout"
	        android:layout_width="match_parent"
	        android:layout_height="match_parent">
	
	        <com.lzy.ui.VerticalScrollView
			    android:id="@+id/custScrollView"
	            android:layout_width="match_parent"
	            android:layout_height="match_parent"
	            android:background="#fff"
	            android:orientation="vertical">
			
			    <LinearLayout
			        android:layout_width="match_parent"
			        android:layout_height="match_parent"
			        android:orientation="vertical">
					
					......

			    </LinearLayout>
			
			</com.lzy.ui.VerticalScrollView>
	
	        <com.lzy.ui.VerticalListView
		        android:id="@+id/listView"
		        android:layout_width="fill_parent"
		        android:layout_height="fill_parent" />

		</com.lzy.ui.DragSlideLayout>
	</RelativeLayout>
```
