---
layout: post
title:  "font-size 的常用长度单位"
author: 
 - name: Xeodou
   link: https://github.com/xeodou
tags:
- css
- font-size
- web design
categories:
- Front-end
---

`font-size` 是网页开发的过程中 CSS 语言针对字体的一个属性，通常我们在设计一个网页时会使用不同的字体大小，可以方便对应 html 中的不同标签，我们一般作为 `Heading` 在整个项目中去不断的复用这些 `Heading`。像诸多设计工具一样，`font-size` 也有不同的单位，设计常用的单位可能有`像素（px）`、`厘米（cm）`等，而在 CSS 中 `font-size` 常用的单位有 `rem`、`em`、`px`、`pt`、`%` 这几种。那么 `font-size` 的不同单位都有什么不同呢？

#### 相对长度单位 `em`、`rem`、`%`

- `em` ，1em 就等于当前元素的字体大小，在印刷媒体时代，通常是用 `M` 的宽度表示 `1em` ，其由来由于英文中的 `--` 与 `M` 是等宽的，`--` 读作 `em-dash`。但是近年 `em` 的定义有点改变，因为很多语言中并没有 `M`（如中文），所以现在表示文字的高度。CSS 中 `em` 的大小计算是根据当前的元素的 `font-size` 来计算最终的字体大小，其父级规定的 `font-size` 对当前元素的真实字体大小影响较为严重。
- `rem` 的大小是根据根元素的（root element）的定义的 `font-size` 来计算最终大小，根元素通常为 HTML 网页中的 `html` 和 `body` 标签。
- `%` 就是百分比，按照当前的字体大小为 `100%` 来计算当前元素最终的字体大小。其特点和 `em` 很像。

~~~ html
<div style="font-size:14px;">
  <span style="font-size: 1.2em;">1.2em</span>
  <span style="font-size: 120%;">120%</span>
  <span style="font-size: 1.2rem;">1.2rem</span>
</div>
~~~

<div style="font-size:14px;text-align:center;">
  <span style="font-size: 1.2em;">1.2em</span>
  <span style="font-size: 120%;">120%</span>
  <span style="font-size: 1.2rem;">1.2rem</span>
</div>

上面的例子中，`div`规定了其子级的 `font-size` 为 `14px`，我们可以看到第一个和第二个 `span` 的字体大小根据当前的字体大小被改变了。而第三个 `span` 的字体大小却是根据根元素的字体大小来计算的。
<br/>

#### 绝对长度单位 `pt`、`px`

- `pt（point）` 是一个物理上的单位，不会随着不同设备的屏幕进行缩放，对于打印设备比较友好，比如我们在使用 CSS 中的 `@medie screen print`。1pt大小等于 1/72 英寸。
- `px` 是一个视觉角度单位，一个像素代表屏幕的一个点。对于一个 72 PPI（pixels per inch)的屏幕来说，1px 就等于 1pt。会随着不同的屏幕进行缩放但是，其长度会受当前屏幕的 PPI 严重影响。举例来说通常你设置一个 `margin` 或者 `pading` 的值你会觉得在不同的屏幕下偏差很多，就是因为屏幕的 PPI 不一样。

~~~ html
<div style="font-size:14px;text-align:center;">
  <span style="font-size: 16px">16px</span>
  <span style="font-size: 16pt;">16pt</span>
</div>
~~~

<div style="font-size:14px;text-align:center;">
  <span style="font-size: 16px">16px</span>
  <span style="font-size: 16pt;">16pt</span>
</div>
上面我们可以看到，虽然我规定了 `div` 下的所有元素字体大小都为 `14px` ，但是其子级的元素都按照其自身的字体的绝对大小显示。
<br/>

#### `em` 和 `px`

近年来网页设计突出响应式设计和 Mobile First 的网页设计模式，由这两点出发我们也应选择相对单位而非绝对单位。而从适配的角度出发我们也更应该选择 `em` 而非 `px` ，对于 `em` 和 `%` 而言没有太多差别只不过 `em` 在语义上更有意义。而且 `em` 不单单可以用来表示字体大小，也可以在 `margin` `padding` 这些凡是涉及距离的属性上使用（`%` 当然也可以不过在默写情况下如 `position: absolute/fixed;` 的时候，`%` 就是相对根元素的百分比了。）。

#### 字体大小

我在上面举例时多次使用`font-size:14px`，其实一般浏览器默认的字体大小为 `16px`[^2]，所以在网页设计时，一般页面的 body 字体大小不会超过 `16px` ，body 字体主要是指网页的大段文字的字体大小，推荐范围是`14px-16px`。很多页面会使用 `16px` 但是从视觉上来说还是有点偏大，很多著名的前端框架，例如 **[bootstrap](https://getbootstrap.com)** 默认使用 `1em=14px` 作为主要字体大小。可以点击下面的两个按钮查看这两个字体大小下对于大段文字的视觉影响。
<div>
  <button class="btn btn-sm btn-blue action-btn" onclick="(function(){$('body').css('font-size','1.6em')})()">16px</button>
  <button class="btn btn-sm btn-blue action-btn" onclick="(function(){$('body').css('font-size','1.4em')})()">14px</button>
</div>
<br/>

#### 总结

在网页的设计和开发过程中，需要考量的因素有很多，从响应式的角度来考虑应该使用相对计量单位，避免使用绝对单位，包括 w3c 也极力推荐网页开发者使用`em`做为常用单位[^1]；从网页的内容来考虑定义字体的大小，越小的字体代表用户最终阅读到的内容越少。而一般我们在设计开发一个网页项目的起始阶段会定义整个项目的 `heading`，如：

~~~ css
html {
  font-size: 62.5%; /* set 1em = 10px; */
}
body {
  font-size: 1.4em; /* initial font-size: 14px; */
}
h1 { font-size: 4.0em; }
h2 { font-size: 3.6em; }
h3 { font-size: 3.0em; }
h4 { font-size: 2.4em; }
h5 { font-size: 1.8em; }
h6 { font-size: 1.5em; }
~~~


**参考链接**: 
    
[^1]: https://www.w3.org/WAI/GL/css2em.htm
[^2]: http://www.w3schools.com/css/css_font.asp
