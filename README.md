# 写在最前
## 个人简介
2020年初，一个偶然的机会，我来到了北京字节跳动，大学期间主要学习开发学校中教学系统，前端和后端都有涉及过，但是入职的岗位是：**Android开发**，就这样开始了学习从Android零基础开始学习。

一年后，2021年对于Android有了简单的理解，知道了这方向涉及的主要有哪些，但是自我感觉到了瓶颈期，不知道如何进行下一步学习，于是有了这么一个想法：通过自研一个App，对自己遇到的问题都做记录，这样使得对自己的能力有更加清晰的认知，同时不断提升自己。

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
