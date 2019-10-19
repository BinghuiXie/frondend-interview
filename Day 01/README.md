Day 01
----------------

## 页面导入样式时，使用link和@import有什么区别？
之前没有仔细考虑过这个问题，这次遇到了，也好好查了一下，做了一下总结：
link 和 @import 大概可以从两个方面来讨论他们的区别
1. 用途
2. 性能
3. 兼容性

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

 link 进来的外部样式表在页面加载的时候就可以开始下载。   
 @import 进来的样式表在页面加载完毕以后才开始下载。   
 
兼容性：
 link 可以被所有的浏览器识别，但是 @import 只能被 IE5+ 的浏览器识别，其他浏览器可以识别 @import

相关链接：
1. [Correctly Using Titles With External Stylesheets](https://developer.mozilla.org/en-US/docs/Archive/Web_Standards/Correctly_Using_Titles_With_External_Stylesheets)
2. [link rel=alternate网站换肤功能最佳实现](https://www.zhangxinxu.com/wordpress/2019/02/link-rel-alternate-website-skin/)
3. [don’t use @import](http://www.stevesouders.com/blog/2009/04/09/dont-use-import/)

## 圣杯布局和双飞翼布局

### 圣杯布局
html代码
```html
<body>
  <div class="container">
    <div class="middle">
      <h4>middle</h4>
    </div>
    <div class="left">
      <h4>left</h4>
    </div>
    <div class="right">
      <h4>right</h4>
    </div>
  </div>
</body>
```
css 代码
```css

    body {
      min-width: 700px;
    }
    .left,
    .middle,
    .right {
      position: relative;
      float: left;
      height: 130px;
    }
    .container {
      padding: 0 220px 0 200px;
      overflow: hidden;
    }
    .left {
      margin-left: -100%;
      left: -200px;
      width: 200px;
      background: red;
    }
    .right {
      margin-left: -220px;
      right: -220px;
      width: 220px;
      background: green;
    }
    .middle {
      width: 100%;
      background: blue;
      word-break: break-all;
    }
```

### 双飞翼布局
html 代码
```html
<body>
    <div class="main">
        <div class="main-inner">
            <h4>main</h4>
        </div>
    </div>
    <div class="sub">
        <h4>sub</h4>
    </div>
    <div class="extra">
        <h4>extra</h4>
    </div>
</body>
```
css 代码
```css
    body{min-width: 700px;}
    .sub,
    .main,
    .extra{ 
        float: left;
        height: 130px;
    }
    .sub{
        margin-left: -100%;
        width: 200px;
        background: red;
    }
    .extra{
        margin-left: -220px;
        width: 220px;
        background: blue;
    }
    .main{ 
        width: 100%;
    }
    .main-inner{ 
        margin-left: 200px;
        margin-right: 220px;
        height: 130px;
        background: green;
        word-break: break-all;
    }
```

圣杯布局和双飞翼，他们的共同点：
1. 都是实现一种左右两栏定宽，中间自适应的布局
2. 桑最外层的容器都使用了浮动，左右两个容器进行负的外边距进行调整
3. 在 html 结构上，都是中间块放在最前面，左右两个块放在后面，这样是为了让中间的块最先加载，因为一般来说内容比较重要
4. body 都需要给一个最小宽度

不同点(参考上面的代码进行对比)：
1. html 结构
    - 外层容器：双飞翼布局直接对三栏三个容器进行布局设定，外层无多余容器，圣杯布局三栏的容器的外层还有一层容器，这层容器宽度自适应，左右内边距留出定宽的两栏，起到限制中间容器宽度的作用。
    - 内层容器：(不考虑内容的前提下)双飞翼布局中间容器内部还需要设置一个容器(div)，这个内层容器左右给出定宽的外边距，留出左右两栏的宽度，而圣杯布局中，这个功能由三栏的最外层容器实现了，所以内部无其他 div
2. css 样式
    - 圣杯布局三栏的 div 除了使用浮动，还需要使用定位( position: relative )，双飞翼不需要
    - 双飞翼布局就算在浏览器宽度缩的再小，最多也就是中间块的宽度被挤没了，三个栏还是会在一行，但是圣杯布局在最外层容器(container)宽度小于左侧栏宽度时，布局会崩掉，变成两行，这是因为，左侧栏需要调整负的左外边距(margin-left: -100%)来达到与父元素在同一行的效果，但是当 container 宽度小于左侧栏宽度时，现在它会被挤到下一行去，所以会出现两行。
    - 圣杯主要使用负的外边距加定位来实现不遮挡中间内容，双飞翼只使用负的外边距
3. 文字
    - 文字方面，圣杯布局因为最外层的 container 包裹三栏，而三栏都需要设置浮动，所以 container 要清除浮动，这里如果使用了 overflow: hidden，那么在 container
    定高的前提下，超出其高度部分的文字都将不可见，双飞翼没这个问题
    
### 用递归算法实现，数组长度为5且元素的随机数在2-32间不重复的值

```javascript
    function foo(arr) {
        if (arr.length === 5) {
            return arr;
        }
        let num = Math.round(Math.random() * 31 + 2);
        arr.push(num);
        arr = [...new Set(arr)];
        return foo(arr);
    }
    
    const res = foo([]);
```
