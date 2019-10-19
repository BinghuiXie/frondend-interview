Day 01
----------------

## 页面导入样式时，使用link和@import有什么区别？
之前没有仔细考虑过这个问题，这次遇到了，也好好查了一下，做了一下总结：
link 和 @import 大概可以从两个方面来讨论他们的区别
1. 用途
2. 性能

先说用途，link 和 @import 都是常用的的导入样式的方式，区别在于，link 是 html 的一个标签，
link 只能用在 html 文档里面，而 @import 只能用在 css 文件(或者 css 预编译语言如 sass，less)文件中，同时
也可以在 style 标签中使用，但是最好将其放在 style 标签中的最上面，但是 link 作为一个 html 标签，它有
很多属性是 @import 不具备的，比如说 rel, title, 这些，而根据这两个属性是否有值，可以将外部样式表分为三类：
1. persistent stylesheet
2. preferred stylesheet
3. alternate stylesheet

persistent stylesheet 是最常见的一种样式表，他不需要设置 title 属性，只需要 rel 属性设置 stylesheet:
```html
<link rel="stylesheet" href="default.css">
```
一个页面中可以有多个 persistent 的样式表，他们默认全部都会起作用，至于哪个覆盖哪个那个就是引入的顺序的事情了。

preferred stylesheet 需要设置 rel 属性为 stylesheet 以及给定一个 title 属性，其值可以任意
```html
<link rel="stylesheet" href="fancy.css" title="preferred stylesheet example">
```
需要注意的是，preferred 外部样式表在一个 html 页面中只能有一个，或者说，就算一个页面中有多个
preferred 样式表，也只会有一个起作用，例如：
```html
<link rel="stylesheet" href="fancy.css" title="preferred stylesheet example01">
<link rel="stylesheet" href="danger.css" title="preferred stylesheet example02">
```
默认情况下，起作用的是 **preferred stylesheet example01**， 而且就算把第一个 link 的 disabled 属性
设置为 true，也就是禁用这个样式，下一个也不会起作用。而且严格的来说，如果设有多个 preferred 的样式表而他们的 title 属性
是一样的话，那么他们都会起作用，其实这个也很好理解， title 一样就会认为是一个(一种)样式表，所以前面说的只有一个 preferred 样式表
起作用应该加一个限制条件：title 不一样的前提下

alternate 样式表需要在 rel 属性里面设置 stylesheet 和 alternate 以及 title 属性
```html
<link rel="alternate stylesheet" href="fancy.css" title="alternate stylesheet example01">
<link rel="alternate stylesheet" href="danger.css" title="alternate stylesheet example02">
```
alternate 样式表默认情况下是不渲染的，这给用于提供了一种切换网页风格的模式，也就是说，可以根据 js 控制
其渲染还是不渲染。
总结一下：
- persistent 的样式表可以在页面中有多个，而且默认全部渲染
- preferred 的样式表默认渲染，但是在一个页面中只能有一类(title一样)
- alternate 的样式表默认**不渲染**，可以通过使用 js 控制器是否渲染

当然其他两类也可以通过 js 控制其 disabled 属性来达到是否渲染的目的

说了这么多，其实就是想说通过 link 可以控制外部样式表是否渲染，而 @import 是不可以的，这在用途上也是一个差别

再说性能：


相关链接：
1. [Correctly Using Titles With External Stylesheets](https://developer.mozilla.org/en-US/docs/Archive/Web_Standards/Correctly_Using_Titles_With_External_Stylesheets)
2. [link rel=alternate网站换肤功能最佳实现](https://www.zhangxinxu.com/wordpress/2019/02/link-rel-alternate-website-skin/)
3. [don’t use @import](http://www.stevesouders.com/blog/2009/04/09/dont-use-import/)