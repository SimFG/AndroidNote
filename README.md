# 学无止境

## Android 工具
- Termux 手机运行Linux命令
- SAI 安装分成多个多apk的App工具
- VMOS 手机运行操作系统
- Xposed Android逆向
- Auto.js 手机执行js脚本
- scrcpy 一个高效手机投屏的Github项目

## 工具网站
- 通过一段文字自动生成流程图：<https://flowchart.fun/>
- 名字变量命名：<https://unbug.github.io/codelf/>
- 图片压缩：<https://tinypng.com/>
- 生成代码片段图片：<https://carbon.now.sh/>
- json格式化：<https://www.json.cn/>
- 图片更换背景：<https://www.qtool.net/bgphoto>
- markdown生成目录：<https://toc.codepie.fun/>

## 有趣小知识点
- chrome 书签脚本：可以在某个书签中写js代码，一键操作网页，参考：<https://www.zhihu.com/question/267011124>
- Github执行定时任务，参考：<https://www.ruanyifeng.com/blog/2019/12/github_actions.html>

## 趣味乐园
- 表情包大全：<https://fabiaoqing.com/>
- 表情包制作、个性化文字：<https://www.wakatool.com/>
- 文字转字符画：<http://patorjk.com/software/taag>
- 图片转字符画：<http://life.chacuo.net/convertphoto2char>
- 前端数据展示库D3：https://observablehq.com/@d3/gallery

## 2021-3-31
### xml中如何取消主题中的某个属性
将值设置为`@null`
### xml中`android:insetTop`含义
可实现以下场景：给一个view设置背景，但是不想这个背景填充整个view，那么就可以用这个属性进行偏移，且保证这个view的大小，如果可以适当减小view的大小，那么用margin就可以。
### 查看MaterialDesign中的组件
- [使用MaterialButton](https://www.jianshu.com/p/bc71b4179cb2)
- [Drawable自定义](https://juejin.cn/post/6844903460987469838)
- [使用ShapeImageView](https://juejin.cn/post/6869376452040196109)

## 2021-3-30
### 寻找SmartSwipe中导致view的requestLayout失效原因
未找到原因，但是看到一篇比较好的文章，[android 设置状态栏文字颜色引起的页面无法重新layout的问题](https://www.jianshu.com/p/b843b537309b)
### 使用AS中模拟器无法调试view的源码
解决办法：下载模拟器镜像选择无google服务，当然前提是模拟器的版本与本地的编译版本一致。

## 2021-3-17
分析[SmartSwipe](https://github.com/luckybilly/SmartSwipe)中的DrawerConsumer，详细见[个人SmartSwipe](https://github.com/SimFG/SmartSwipe)

## 2021-3-15
### RecyclerView中onTouch不执行DOWN事件
参考： <https://www.jianshu.com/p/a4dc607cd08f>

## 2021-3-14
分析[SmartSwipe](https://github.com/luckybilly/SmartSwipe)
### 调试SmartSwipe-在StretchConsumerActivity中，点击事件最开始会调用onStartNestedScroll方法三次
- 事件发起是在RecyclerView中，onInterceptTouchEvent
- 第一次进入onStartNestedScroll，是RecyclerView的父类
- 其实当前布局中存在两个SmartSwipeWrapper，一个是RecyclerView的父类，另外一个是DecorView的第一个子类
- 这个方法会进入最后的handle为true，会调用startNestedScroll，这个是第二次进入onStartNestedScroll
- 最后一次进入是因为在第一次onStartNestedScroll方法返回了true，调用的onNestedScrollAccepted方法，然后导致了第三次进入onStartNestedScroll
### 为什么DecorView的第一个子类是SmartSwipeWrapper呢？
- 这个是在MyApp中onCreate中进行注册的，即SmartSwipeBack#activityBezierBack

## 2021-3-12
### 再次理解内嵌滑动
参考：<https://blog.csdn.net/King1425/article/details/61915758/>

## 2021-3-11
### NestedScrolling解析
简介：view的touch事件分发机制是自上而下的，其touch事件要么被父View处理，要么被子View处理，很难在两者之间进行交互处理。但Google提供的NestedScrolling机制能够很好地解决这个问题。
参考：
- <https://www.jianshu.com/p/eeae65885043>
- <https://www.jianshu.com/p/20efb9f65494>
注：AndroidX中NestedScrollView其实就是以上的官方实现

## 2021-3-10
### DrawerLayout与SlidingPaneLayout
DrawerLayout：<https://www.jianshu.com/p/0026621c70bc>

SlidingPaneLayout：<https://zhuanlan.zhihu.com/p/234598182>

区别：<https://www.codenong.com/cs107088957/>

### Activity侧滑退出
简介：将“测滑退出”分为两个过程：实现当前Activity跟随手指进行滑动；展现底部（上一级）Activity的view，并对其进行相应操作（各种动画）。
参考：
- <https://www.jianshu.com/p/1a2d2ce6e8f2>
- <https://juejin.cn/post/6844903902937088013#heading-22>

## 2021-3-9
### adb安装，报错INSTALL_FAILED_TEST_ONLY
解决方案：项目根目录下，配置gradle.properties配置项
```
android.injected.testOnly=false
```
参考：<https://www.cnblogs.com/lwbqqyumidi/p/10217076.html>

### OverScroller和Scroller有什么区别呢
两个类都属于Scrollers，Scroller出现的比较早，在API1就有了，OverScroller是在API9才添加上的，出现的比较晚，所以功能比较完善，OverScroller提供了对超出滑动边界的情况的处理，这两个类80%的API是一致的，OverScroller比Scroller额外添加了几个方法，包括了：isOverScrolled(),springBack(),fling(),notifyHorizontalEdgeReached(),notifyVerticalEdgeReached()。
参考：<https://blog.csdn.net/zhaokaiqiang1992/article/details/43986365>

### Canvas理解
参考：<https://blog.csdn.net/carson_ho/article/details/60598775>

## 2021-3-8
### 保存图片至相册
在oneplus 7T pro(android 10)和vivo Z1(Android 9)上已测试
```
fun saveMediaToStorage(bitmap: Bitmap) : Uri? {
    //Generating a file name
    val filename = "${System.currentTimeMillis()}_maker.jpg"

    //Output stream
    var fos: OutputStream? = null

    var imageUri: Uri? = null

    //For devices running android >= Q
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.Q) {
        //getting the contentResolver
        context?.contentResolver?.also { resolver ->

            //Content resolver will process the contentvalues
            val contentValues = ContentValues().apply {

                //putting file information in content values
                put(MediaStore.MediaColumns.DISPLAY_NAME, filename)
                put(MediaStore.MediaColumns.MIME_TYPE, "image/jpg")
                put(MediaStore.MediaColumns.RELATIVE_PATH, Environment.DIRECTORY_PICTURES)
            }

            //Inserting the contentValues to contentResolver and getting the Uri
            imageUri = resolver.insert(MediaStore.Images.Media.EXTERNAL_CONTENT_URI, contentValues)

            //Opening an outputstream with the Uri that we got
            fos = imageUri?.let { resolver.openOutputStream(it) }
        }
    } else {
        //These for devices running on android < Q
        //So I don't think an explanation is needed here
        val imagesDir =
                Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_PICTURES)
        val image = File(imagesDir, filename)
        imageUri = Uri.fromFile(image)
        fos = FileOutputStream(image)
    }

    fos?.use {
        //Finally writing the bitmap to the output stream that we opened
        bitmap.compress(Bitmap.CompressFormat.JPEG, 100, it)
        ToastUtils.showShort("图片已保存到相册")
    }
    context?.sendBroadcast(Intent(Intent.ACTION_MEDIA_SCANNER_SCAN_FILE, imageUri))

    return imageUri
}
```
注：需要申请相册权限
```
ActivityCompat.requestPermissions(activity!!, arrayOf(Manifest.permission.WRITE_EXTERNAL_STORAGE), 1000)
```

## 2021-3-5
### requestDisallowInterceptTouchEvent含义
当传入的参数为true时，表示子组件要自己消费这次事件，告诉父组件不要拦截（抢走）这次的事件。参考：<https://blog.csdn.net/shineflowers/article/details/48319825>

### 剪切板
简单使用
```
//获取剪贴板管理器：
ClipboardManager cm = (ClipboardManager) getSystemService(Context.CLIPBOARD_SERVICE);
// 创建普通字符型ClipData
ClipData mClipData = ClipData.newPlainText("Label", "这里是要复制的文字");
// 将ClipData内容放到系统剪贴板里。
cm.setPrimaryClip(mClipData);
```

### 获取View的截图Bitmap两种方式
```
fun viewToBitmap(view: View) : Bitmap {
    val bitmap = Bitmap.createBitmap(view.width, view.height, Bitmap.Config.ARGB_8888)
    val canvas = Canvas(bitmap)
    view.draw(canvas)
    return bitmap
}

fun viewToBitmap2(view: View) : Bitmap {
    view.isDrawingCacheEnabled = true
    view.buildDrawingCache()
    val bitmap = view.drawingCache
    view.isDrawingCacheEnabled = false
    return bitmap
}
```

## 2021-3-4
### 很多View中在androidx都对应了一个CompatView，比如ImageView对应AppCompatImageView，这是什么意思？
参考：<https://www.jianshu.com/p/090fb849f81a>

### ImageView中的scaleType温习
参考：<https://blog.csdn.net/u013325929/article/details/50624140>

## 2021-3-2
### 对于自定义的组合组件，即类似于直接继承RelativeLayout的自定义View，两种布局方式：
1. 使用xml进行自定义布局
注：使用inflater加载xml，建议使用merge为根节点，或者保存inflater返回的对象作为rootView
2. 使用addView添加布局
### ViewDragHelper的一些问题
需求背景：有一个ViewGroup，可以添加图片，图片被添加可以拖拽，同时点击的时候图片可以被一个自定义View包围，自定义View中间透明，四个角有四个小方块。
1. 往ViewGroup中添加View后所有被拖动的view位置都重置了
解决方案：
- View被拖动的时候记录一下，在ViewDragHelper的回调方法中onViewReleased进行记录；
- 重写ViewGroup的onLayout方法，使用View的offsetLeftAndRight进行偏移；
2. 自定义View被显示的时候，在图片被拖拽的时候进行跟随
解决方案：
- 点击图片的时候，显示View，和上面相同的方式存储View的位置
- 拖拽的时候，重写ViewDragHelper的回调方法中onViewPositionChanged方法，调用View的offsetLeftAndRight进行偏移
3. 添加自定义View的时机，如果在第一次点击的时候添加，发现添加后View无法拖动
原因：因为添加的View被盖在图片上了，其index大于图片，所以ViewDragHelper事件传递是index从大到小依次检测，检测到自定义View的时候事件被丢弃，因为自定义View只能跟随图片拖动，不能自己单独拖动；
解决方法：在ViewGroup创建时就添加自定义View，设置其不可见，点击的时候设置其可见即可；

## 2021-3-1
### View刷新的方法
1. requestLayout，会导致onMeasure和onLayout被调用，但不一定会触发OnDraw；
2. invalidate，不会导致onMeasure和onLayout被调用，而OnDraw会被调用；
3. postInvalidate是在非UI线程中调用，invalidate则是在UI线程中调用；
4. postInvalidate方法是将任务添加到队列中排队后立即执行的，而postInvalidateOnAnimation依赖上一帧动画的的执行时间，因为动画的刷新是存在一个频率的，直到下一帧动画的时间才会真正执行刷新操作。

- 对于postInvalidateOnAnimation调用可以使用：ViewCompat.postInvalidateOnAnimation()；
- 只要刷新的时候调用invalidate，需要重新measure就调用requestLayout，建议后面再跟个invalidate（为了保证重绘）；
- 对于View的一些操作可以考虑使用ViewCompat
### 理解Scroller
参考：<https://blog.csdn.net/qinjuning/article/details/7419207>

可以理解View中的computeScroll方法含义

## 2021-2-28
### 阅读ViewDragHelp源码
参考：<https://www.cnblogs.com/lqstayreal/p/4500219.html>
1. 解决整个ViewGroup中的子View无法拖动
查看子View的clickable属性是否为true，为true要重写ViewDragHelper.Callback类中的getViewVerticalDragRange和getViewHorizontalDragRange方法
2. 拖动完成后还要对View的位置进行调整，比如说吸边效果
重写ViewGroup的computeScroll方法
```
override fun computeScroll() {
    if (mViewDragHelper.continueSettling(true)) {
        ViewCompat.postInvalidateOnAnimation(this)
    }
}
```
3. 拖拽过程中调整View的位置
在回调方法中onViewReleased可以使用settleCapturedViewAt方法，其他地方使用smoothSlideViewTo()方法
4. 设置整个ViewGroup为可点击失效
重写ViewGroup的onTouchEvent方法。
override fun onTouchEvent(event: MotionEvent): Boolean {
    mViewDragHelper.processTouchEvent(event)
    if(mViewDragHelper.capturedView != null) {
        return true
    }
    return super.onTouchEvent(event)
}
