@[TOC](响应式布局)

# 是什么，为什么？
电脑显示器有很多种尺寸，常见的有，1366px（宽度）、1980px（宽度），高级点，还有 2k 屏、4k 屏、5k 屏。如果再考虑平板和手机的话，就更多了。  
远古时期，大多数网站是不会考虑这个问题的，一般只会对特别流行的一个尺寸进行设计。这时候，屏幕特别小，或者特别大时，看着就很不舒服。  
为了照顾小屏幕、高分屏（高分辨率屏），就有人提出来“响应式”这种概念。

笼统来讲，使不同尺寸、分辨率的屏幕，都能呈现出**较好**的效果，就是**响应式布局**。

 > 还有一个概念叫“自适应”，从目的上讲，和“响应式”一样。但是码者不是一个那么严谨的人，这里就混作一谈来。

本文记录码者开发中使用过的几种**自适应思路**供同仁参考、讨论，以及批评。

# 最简单的方法（固定内容区宽度）
最简单的方法，就是**给网页的内容区，设置一个宽度**。比如 900px，然后内容区居中、两边留白。因为大部分电脑显示器的宽度都超过 900px，所以，不同的显示器的差异，仅仅在于两边“留白”的大小。  
虽然很简单，但是确实效果不错。码者比较推荐这种方法，但可能是因为简单，很多人不屑于使用这种方法。

# 同比例缩放（rem）
这种方法的思路就是“大屏幕的字大点、小屏幕的字小点”。  
其实这是一个**看起来很合理**的方法，但是在码者看来是今天所讨论的几个方法中，最 low 的方法。

因为很多时候，很难判定屏幕**是大是小**。  
大与小，可以是像素的多与少，比如，1920px 宽的屏幕，比 1366px 宽的屏幕“大”；  
但是，1920px 的屏幕有可能是 13 寸的，而 1366px 的屏幕有可能是 15.6 寸的。  
也就是说，物理尺寸大的屏幕，像素尺寸可能更低。也就会出现这种情况：13 寸显示器上的字体比 15.6 寸的还小。实际体验上，有更多你想不到的 low 点。

但是在某些场景，还是很受用的。所以这里记录**同比例缩放**的一种实现方案——rem 方案。

> 不熟悉 rem 的同仁，可以参考[这个简介](https://blog.csdn.net/csdn372301467/article/details/105910813)

### 整体思路
+ 给根节点设置一个字体大小
+ 然后，其他尺寸单位全部用 rem
+ 然后，监听屏幕的大小
+ 然后，根据屏幕的大小**按比例改变**根节点字体的大小

因为 rem 的特性，其他的尺寸都是根据根节点字体的大小设定的，所以，根节点字体大小一变，其他所有尺寸都会按比例变大、或小。

> 大部分浏览器的字体有个最小值：12px。也就是缩小到 12px 后，字体不会继续缩小。

### demo
```html
<style>
.target{
  width: 3rem;
  height: 3rem;
  background-color: red;
  font-size: 0.36rem;
}
</style>

<div class="target">responsive</div>

<script>
function initResponsize(){
  const root = document.getElementsByTagName('html')[0] // 根元素
  // 根据屏幕宽度，设置根元素字体大小的函数
  function setRootFontSize(){
    // 1920px 宽的屏幕上，字体大小设置成 100px，其他尺寸按比例……
    root.style.fontSize = root.clientWidth/1920 * 100
  }
  setRootFontSize()

  // 屏幕大小改变时，重新设置根元素字体大小，如果确定屏幕大小不变，或者不屑于处理屏幕变化，可以不写
  window.addEventListener('resize', setRootFontSize)
}
initResponsize()
</script>
```

# 最精准（媒体查询，@media）
### 整体思路
```@media```起到的作用是，你可以**给不同尺寸的设备，设置单独、互不影响的 css 属性**。  
+ 比如：屏幕宽度小于 500px 时（手机屏幕），怎样怎样
+ 宽度在 500px ～ 1000px 时（平板屏幕），怎样怎样
+ 宽度在 1000px ～ 1500px 时（电脑小屏幕），怎样怎样
+ 宽度在 1500px ～ 2000px 时（普通电脑屏幕），怎样怎样
+ 宽度在 ……

### demo
```css
@media (max-width: 500px){
  .target{
    width: 100px
  }
}
@media (min-width: 500px and max-width: 1000px){
  .target{
    width: 50px;
  }
}
```