---
title: 块格式化上下文（Block Formatting Context，BFC）
date: 2017-11-18 19:22:45
tags:
- CSS
---

## 概述
块格式化上下文（Block Formatting Context，BFC） 是Web页面的可视化CSS渲染的一部分，是布局过程中生成块级盒子的区域，也是浮动元素与其他元素的交互限定区域。
BFC元素特性表现原则是：具有 BFC 特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且 BFC 具有普通容器所没有的一些特性。
通俗来讲，可把 BFC 理解为一个封闭的大箱子，箱子内部的元素无论如何翻江倒海，都不会影响到外部。

<!--more--> 

## 什么时候会触发BFC？
* 根元素或包含根元素的元素
* 浮动元素（元素的 float 不是 none）
* 绝对定位元素（元素的 position 为 absolute 或 fixed）
* 行内块元素（元素的 display 为 inline-block）
* 表格单元格（元素的 display为 table-cell，HTML表格单元格默认为该值）
* 表格标题（元素的 display 为 table-caption，HTML表格标题默认为该值）
* 匿名表格单元格元素（元素的 display为 table、table-row、 table-row-group、table-header-group、table-footer-group（分别是HTML table、row、tbody、thead、tfoot的默认属性）或 inline-table）
* overflow 值不为 visible 的块元素
* display 值为 flow-root 的元素
* contain 值为 layout、content或 strict 的元素
* 弹性元素（display为 flex 或 inline-flex元素的直接子元素）
* 网格元素（display为 grid 或 inline-grid 元素的直接子元素）
* 多列容器（元素的 column-count 或 column-width 不为 auto，包括 column-count 为 1）
创建了块格式化上下文的元素中的所有内容都会被包含到该BFC中。
块格式化上下文对浮动定位（参见 float）与清除浮动（参见 clear）都很重要。浮动定位和清除浮动时只会应用于同一个BFC内的元素。浮动不会影响其它BFC中元素的布局，而清除浮动只能清除同一BFC中在它前面的元素的浮动。外边距折叠（Margin collapsing）也只会发生在属于同一BFC的块级元素之间。

## BFC 特性及应用
### 同一个 BFC 下外边距会发生折叠
```html
<style>
  <style>
  div{
    width: 100px;
    height: 100px;
    background: #f00;
    margin: 100px;
  }
  </style>
  <div></div>
  <div></div>
```
{% asset_img pic1.png %}
因为两个 div 元素都处于同一个 BFC 容器下 (这里指 body 元素) 所以第一个 div 的下边距和第二个 div 的上边距发生了重叠，所以两个盒子之间距离只有 100px，而不是 200px。

如果想要避免外边距的重叠，可以将其放在不同的 BFC 容器中。
```html
  <style>
  .wrap {
    overflow: hidden;
  }
  .con {
    width: 100px;
    height: 100px;
    background: #f00;
    margin: 100px;
  }
  </style>
  <div class="wrap">
    <div class="con"></div>
  </div>
  <div class="wrap">
    <div class="con"></div>
  </div>
```
{% asset_img pic2.png %}

### BFC 可以包含浮动的元素（清除浮动）
浮动的元素会脱离普通文档流，如下：
```html
  <div style="border: 1px solid #000;">
    <div style="width: 100px;height: 100px;background: #f00;float: left;"></div>
  </div>
```
{% asset_img pic3.png %}

由于容器内元素浮动，脱离了文档流，所以容器只剩下 2px 的边距高度。如果使触发容器的 BFC，那么容器将会包裹着浮动元素。
```html
  <div style="border: 1px solid #000;overflow: hidden;">
    <div style="width: 100px;height: 100px;background: #f00;float: left;"></div>
  </div>
```
{% asset_img pic4.png %}

### BFC 可以阻止元素被浮动元素覆盖
先来看一个文字环绕效果：
```html
  <div style="height: 100px;width: 100px;float: left;background: #f00">我是一个左浮动的元素</div>
  <div style="width: 200px; height: 200px;background: #eee">我是一个没有设置浮动, 也没有触发 BFC 元素, width: 200px; height:200px; background: #eee;</div>
```
{% asset_img pic5.png %}
这时，第二个元素有部分被浮动元素所覆盖，(但是文本信息不会被浮动元素所覆盖) 如果想避免元素被覆盖，可触第二个元素的 BFC 特性，在第二个元素中加入 overflow: hidden，就会变成：
{% asset_img pic6.png %}
该方法可以用来实现两列自适应布局，效果不错，这时候左边的宽度固定，右边的内容自适应宽度(去掉上面右边内容的宽度)。

理论上，任何BFC元素和浮动交互时，都可以实现自动填充的自适应布局。
但是，由于绝大多数的触发BFC的属性自身有一些古怪的特性，所以，实际操作的时候，能兼顾流体特性和BFC特性来实现自适应布局的属性并不多。
由于overflow有剪裁和出现滚动条等隐患，不适合作为整站通用类，因此，类似清除浮动的通用类语句：
```html
  <style>
    .clearfix {
      *zoom: 1;
    }
    .clearfix:after {
      content: ''; display: table; clear: both;
    }
  </style>
  <div class="clearfix" style="border: 1px solid #000;">
    <div style="width: 100px;height: 100px;background: #f00;float: left;"></div>
  </div>
```