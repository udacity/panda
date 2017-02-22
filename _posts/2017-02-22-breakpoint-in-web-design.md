---
layout: post
title:  "Web 设计中的 break point"
author: 
 - name: Xeodou
   link: https://github.com/xeodou
tags:
- css
- break point
- web design
categories:
- Front-end
---

由于移动设备（手机，平板电脑等）的流行，越来越多的网站开始使用响应式设计来设计网站。其核心归结为一句话就是，在不同设备上自动适配不同的内容（*如下图所示*）。而我们为了让网站样式能够支持响应式设计，其中最关键的因素就是 CSS 中的 **[media queries](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries)**，media queries 允许我们定义在不同内容和尺寸的设备上的样式。
![图片来源：维基百科](https://cloud.githubusercontent.com/assets/914595/21479234/86246a54-cb8d-11e6-99be-fc0160fa31ea.png)

#### 如何使用 Media Queries
上面我们说到 media queries 可以帮助我们定义不同尺寸设备上的内容显示，那么我们只需要在我们现有的样式中增加针对特定内容在特定设备或者尺寸的样式即可，例如：

~~~ CSS
div.container {
  width: 100%;
}
@media screen only and (min-width: 480px) {
  div.container {
    width: 40%;
  }
}
~~~

上面的例子中，我们的定义了只在屏幕（screen only）宽度最小值大于 `480px` 的时候将宽度从 100% 变成 40%，而这个`480px`就是我们通常所说的 **Break Point** 。其中关于更多的 media queries 的属性可以去参考下 Mozilla 的文档[^1]。
不过这个时候大家可能会有所疑问，这里的 `480px` 真的会预期一样么？
![image](https://cloud.githubusercontent.com/assets/914595/21479672/3a28eb80-cb91-11e6-8326-24dabf7fa633.jpg)

#### Break Point 介绍

首先我们要知道，上面的例子中 media queries 针对的是内容的宽度，而通常一个父级元素的宽度是由它包含的子类元素确定的，当然我们也可以制定一个绝对值。当元素的宽度超过屏幕的宽度时我们的内容就会出现水平可滚动的效果，类似下面这种效果：

<div style="width:280px;border: 1px solid black;white-space: nowrap;
overflow-x:scroll;margin: 0 auto;">
 Parent Width 280px
 <div style="width:380px; text-align:center;">Child width 380px</div>
</div>
<br>
同样高度也会出现类似的情况，但是一般从网页交互和用户体验的角度来考虑，我们不会对特定的高度做限定，因为网页的内容是自上而下滚动的，高度可以自由延展。这是可能会想那我定义个 `max-width:280px` 不就好了嘛。是的，这确实解决了我们的问题，但是随之而来的问题是如果用户改变了他的浏览器的默认字体大小怎么办？在我的上一篇博客中「[font-size 的常用长度单位](https://xeodou.me/2016/12/22/introduce-font-size-units/)」我介绍了 CSS 中的几种常见单位，其中的单位对于我们做响应式设计中依然至关重要，那具体是怎么表现的呢？

<p data-height="265" data-theme-id="0" data-slug-hash="xRNeqo" data-default-tab="css,result" data-user="xeodou" data-embed-version="2" data-pen-title="breakpoint" class="codepen">See the Pen <a href="http://codepen.io/xeodou/pen/xRNeqo/">breakpoint</a> by xeodou (<a href="http://codepen.io/xeodou">@xeodou</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

在这个例子中当宽度变化时，`div` 标签的背景透明度将发生改变，我们将从不同维度来看break point 的变化：

* 更改系统字体大小
* 缩放屏幕大小
* 不同浏览器  

*注：我使用的浏览器版本为 Chrome Version 55.0.2883.95 (64-bit) 和 Safari Version 10.0.2 (12602.3.12.0.1)*

###### 正常状态
* Chrome 
![Chrome](https://cloud.githubusercontent.com/assets/914595/21481257/8834017a-cba0-11e6-88e1-e1e564c1a710.gif)
我们可以看到在我拖拽的过程中，随着宽度的缩小三个`div`标签的背景颜色同时变化，那是因为在不改变系统字体大小并且指定`html { font-size: 62.5%; }`的时候，在 Chrome 下
`480px = 30em = 30rem`。

* Safari
![Safari](https://cloud.githubusercontent.com/assets/914595/21481256/8646660a-cba0-11e6-84fe-6acb14ec178a.gif)
我们可以看到其显示的效果和 Chrome 下却有所不同，在宽度小于`480px=30em`时红色块和绿色块颜色透明度减小，而当宽度小于`300px=30rem`时蓝色块才开始变化。

###### 更改字体大小
* Chrome

Chrome 下可以通过`设置`>`高级设置`>`网页内容` 更改字体大小，我们将字体大小从`Medium`更改到`Large`，这时 Chrome 的页面内容正常情况下`1rem=20px`，而当加载`html { font-size:62.5%; }`后字体大小变成`1rem=12.5px`。
![Chrome 1rem=12.5px](https://cloud.githubusercontent.com/assets/914595/21491981/96bef51a-cc3b-11e6-8d45-b9c1f209cb95.gif)
我们可以看到其中字体明显变大了，这时候红色色块依然在宽度小于`480px`的时候颜色变化，而绿色和蓝色色块都同时在`30rem=30em=600px`时候颜色发生变化。

* Safari

Safari 下通过点开`视图（View）`菜单后按住`Option`键后点击放大或者缩小页面字体[^2]。这时我们将字体像 Chrome 下一样增加字体大小后`1rem=19.2px`，而当加载`html { font-size:62.5%; }`后字体大小变成`1rem=12px`。
![Safari](https://cloud.githubusercontent.com/assets/914595/21492438/d078fe1e-cc3f-11e6-82e3-f659ed443e14.gif)
文字大小和 Chrome 下一样都明显被放大了，而这时候红色色块也一样在宽度小于`480px`的时候改变了颜色，而绿色色块在宽度小于`30em=12*1.6*30px=576px`时候改变了颜色，蓝色块在宽度小于`30rem=12*30px`的时候颜色发生变化。

#### 定义 Break Point
从上面的这几个例子我们可以看到，就算在同一个设备不同的浏览器下不同的单位也会有不同的表现，虽然`px`在不同的浏览器甚至是不同的设备中的表现都是一样的，但是当用户希望改变页面的展示形式（字体大小）或者在一个不同的设备上时，他所希望的就是他浏览的网页随着这种改变而改变，所以我们不应该选取`px`这种非响应式的单位，而且不同浏览器下`rem`的定义不一致导致我们很难对最终的宽度很有个预测，因此建议选取`em`做为单位。如果我们在使用 [SASS](http://sass-lang.com/) 这种预编译 CSS 时，我们可以使用类似 [sass-mq](https://github.com/sass-mq/sass-mq) 这种工具库去方便使用各种 media queries，如：

~~~ CSS
$mq-breakpoints: (
  mobile:  20em,
  tablet:  46.24em,
  desktop: 61.25em,
  wide:    81.25em
);
@import 'mq';
.foo {
  @include mq($from: mobile, $until: tablet) {
    background: red;
  }
  @include mq($from: tablet) {
    background: green;
  }
}
~~~

**参考链接**: 

[^1]: [https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries)
[^2]: [https://support.apple.com/en-us/HT207209](https://support.apple.com/en-us/HT207209)
