Day 02
-------------

## html的元素有哪些（包含H5）？
按照不同的分类：
1. 常见块级元素
    - div
    - p
    - ul ( ol ), li
    - table (tr, td, tbody, thead, tfoot 不是块级元素)
    - h1 ~ h6
    - blockquote
    - section
    - aside 
    - main
    - header
    - footer
    - nav
    - body
    - html
    - dl, dt, dd
    
2. 常见行内(内联)元素
    - a
    - i
    - span
    - b
    - strong

3. 替换元素(常见)
    - input
    - video
    - canvas
    - button
    - audio
    - select
    - img
    - iframe
    
4. H5 新增的一些标签有：
    - canvas
    - audio
    - video
    - main, section, nav, footer, header, aside
    - progress
   
相关链接：    
[https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/HTML5/HTML5_element_list](https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/HTML5/HTML5_element_list)   
[https://www.w3.org/TR/CSS2/visuren.html#block-formatting](https://www.w3.org/TR/CSS2/visuren.html#block-formatting)   
[https://www.w3.org/TR/CSS2/visuren.html#block-level](https://www.w3.org/TR/CSS2/visuren.html#block-level)   


## CSS3有哪些新增的特性
- CSS3 新特性
    - 圆角 ( border-radius )
    - 图片边框 ( border-image-*( source, repeat, slice, width) )
    - 多背景 ( Multi Background ): background-image 属性可以设置多个 url，达到多背景的目的
    - 新的颜色值
        - rgba
        - hsl
        - hsla
    - 渐变 ( gradients )
        - 线性渐变: linear-gradient
        - 径向渐变: radial-gradient
    - 阴影 
        - box-shadow
        - text-shadow
    - 文字的一些属性 ( 列举常用 )
        - word-break
        - text-emphasis
        - text-overflow
        - word-wrap
    - 字体 ( font, @font-face )
        - svg
        - ttf
        - woff ...
    - 过渡 ( transform )
        - 2d
            - matrix
            - translate
            - scale
            - skew
        - 3d
            - ...
    - 动画 ( animation )
    - box-sizing
    - 变量 ( --baseColor, var(--baseColor) )
    
相关链接：  
[https://www.tutorialspoint.com/css/css3_tutorial.htm](https://www.tutorialspoint.com/css/css3_tutorial.htm)

## 写一个方法去掉字符串中的空格
```javascript
    function manualTrim(param) {
        let sucMsg = {}, errMsg = {}, temp = param;
        let str = param;
        const centerHasSpace = new RegExp("(\\w+)(\\s+)(\\w+)", "g");
        if (typeof str !== "string") {
            errMsg.isSuccess = false;
            errMsg.msg = 'need string';
            return errMsg;
        }
        sucMsg.removeLeft = str.replace(/^\s+/g, "");
        sucMsg.removeRight = str.replace(/\s+$/g, "");
        sucMsg.removeBothLeftandRight = str.trim();
        sucMsg.removeAll = str.replace(/\s/g, "");
        while (temp.match(centerHasSpace)) {
            temp = temp.replace(centerHasSpace, "$1$3")
        }
        sucMsg.removeCenter = temp;
        sucMsg.msg = "remove space success";
        return sucMsg;
    }
```
函数要求能去掉字符串的左边，右边，中间，左边和右边还有全部的空格，我觉得去掉中间的空格是最麻烦的，大概逻辑是这样的：
首先用到的正则表达式是：**(\w+)(\s+)(\w+)**。  
用一个字符串来举例：  **"    b    l   u    e    "**   
上面的正则用来匹配下面的这个字符串，会找到两个符合结果的匹配： **b    l**, **u    e**  
这样，在使用 replace 方法的时候，会将匹配到的字符替换为我们需要的结果，这里面需要替换的结果是 **$1$3**， **$1** 和 **$3** 代表的是上面正则第一个括号对和第三个括号对匹配到的内容也就是单词 （b,l或u,e）   
所以第一次使用 replace，会得到:
```javascript
const centerHasSpace = new RegExp("(\\w+)(\\s+)(\\w+)", "g");
// 第一次循环 最开始 temp = "    b    l   u    e    "
temp = temp.replace(centerHasSpace, "$1$3")
// 得到 temp = "    bl   ue    "
```
这样就去掉了 b 和 l， u 和 e 之间的空格，同理，第二次循环，去掉的就是 bl 和 ue 之间的空格。   
根据 match 方法，会得到一个匹配满足的数组，使用 while 循环，上面的逻辑会一直进行，
直到 match 匹配不到结果的时候结束循环，这是就已经去掉所有中间的空格了。
如果文字不好理解的话，我弄了一个 gif 图，希望能帮助理解：
![](http://binghuixie.cn/frondend-interview/Day%2002/manual-trim.gif)
        