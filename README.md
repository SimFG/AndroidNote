# 写在最前
## 个人简介
2020年初，一个偶然的机会，我来到了北京字节跳动，大学期间主要学习开发学校中教学系统，前端和后端都有涉及过，但是入职的岗位是：**Android开发**，就这样开始了学习从Android零基础开始学习。

一年后，2021年对于Android有了简单的理解，知道了这方向涉及的主要有哪些，但是自我感觉到了瓶颈期，不知道如何进行下一步学习，于是有了这么一个想法：通过自研一个App，对自己遇到的问题都做记录，这样使得对自己的能力有更加清晰的认知，同时不断提升自己。

## 2021-3-2
## 对于自定义的组合组件，即类似于直接继承RelativeLayout的自定义View，两种布局方式：
1. 使用xml进行自定义布局
注：使用inflater加载xml，建议使用merge为根节点，或者保存inflater返回的对象作为rootView
2. 使用addView添加布局
## ViewDragHelper的一些问题
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
