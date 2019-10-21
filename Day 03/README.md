Day 03
------------

## ( html )HTML全局属性(global attribute)有哪些（包含H5）？
HTML 的全局属性是对所有 html 标签都有的属性，他们可以被用于所有的 html 标签( 有的可能不会起作用 )( 也包括非标准的标签，比如说自定义标签也支持这些属性 )。   

- HTML 全局属性有：
    - aria-* ( 提升页面的可读性，服务于 web accessibility )
    - 事件处理 on* ( onclick, onfocus, onmouseover ... )
    - class
    - id
    - lang
    - style
    - title
    - accesskey: 提供一个通过键盘组合建来快速达到点击某一个元素的效果
    - autocapitalize: 控制文本输入在用户输入/编辑时是否自动以及如何自动大写
    - contenteditable: 控制页面上的元素是否可以修改
    - draggable: 指示元素是否可以使用 Drag 和 Drop API 拖动元素
    - hidden
    等
    
参考链接：
[https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes)

## ( css )在页面上隐藏元素的方法有哪些？

1. `display: none`
2. `visibility： hidden`
3. `opacity: 0`
4. 对应元素的 html 标签上添加 hidden 属性
5. `width: 0; height: 0; overflow: hidden`
6. 父元素 `overflow: hidden`，同时子元素 `margin-left: -100%`，而且，这种情况有限制：
    - 子元素不能设置**右浮动** ( margin 的方向与 float 的方向相反，就不起作用 )(我自己只试出来这种情况下不行)
7. 如果子元素是内联元素或者 inline-block 元素，可以在父元素上设置 `text-indent: -9999px` 使其超出屏幕范围，如果屏幕需要左右滑动，那么最好在父元素上加一个 `overflow: hidden`

暂时想到的就这么多
## ( js )去除字符串中最后一个指定的字符

```javascript
    function removeItem(str, target) {
        if (typeof str !== 'string') {
            throw Error("first parameter has to be a string");
            return ;
        }
        let temp = reverse(str);
        temp = temp.split("");
        for (let i = 0; i < temp.length; i++) {
            if (temp[i] === target) {
                temp.splice(i, 1);
                break;
            }
        }
        temp = temp.join("");
        console.log(temp);
        return reverse(temp);
    }

    function reverse (str) {
        return str.split("").reverse().join('');
    }

    console.log(removeItem("blur click compare", "c"));
```