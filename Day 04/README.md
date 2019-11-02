Day 04
------------------

最近因为考试还有学校的项目，好长时间没写面试题了，今天恢复。

## 第4天 HTML5的文件离线储存怎么使用，工作原理是什么？

首先，离线存储允许我们把数据存在浏览器中一段时间(在断网的时候)，这样，万一断网了，还可以从缓存中拿过来数据

- html5 离线缓存技术
    1. sessionStorage
    2. localStorage
    3. manifest
    
### sessionStorage

sessionStorage 是 html5 的一种浏览器缓存技术，以 **key - value** 的形式存储数据，sessionStorage 中的数据只存储在当前页面，页面关闭后 sessionStorage 中的数据会被删除，
sessionStorage 中存储数据的最大量为 5MB。
设置 sessionStorage:
```javascript
function setSessionStorage(key, value) {
  sessionStorage.setItem(key, value)
}
```
获取 sessionStorage 中的数据
```javascript
function getSessionStorage(key) {
  // 有两种方式获取 sessionStorage 中的数据
  // 第一种
  console.log(sessionStorage.getItem(key));
  // 第二种
  console.log(sessionStorage[key]);
}
```
删除 sessionStorage 中的数据
```javascript
function deleteSessionStorage(key) {
  // 两种方式删除 sessionStorage 中对应的数据
  // 第一种
  sessionStorage.removeItem(key);
  // 第二种
  delete sessionStorage[key];
}
```

### localStorage
localStorage 和 sessionStorage 差不多，只是容量最大为 10MB，其余的删除，新增，获取的 API 都与 sessionStorage 一样，与 sessionStorage 一点不同的是，localStorage
是不会过期的，除非手动在浏览器中删除或者用 js 删除，不像 sessionStorage 那样关了当前页面就被删除了。

localStorage 的操作(设置，获取，删除)
```javascript
// 设置
function setLocalStorage(key, value) {
  localStorage.setItem(key, value)
}
// 获取
function getLocalStorage(key) {
  // 有两种方式获取 sessionStorage 中的数据
  // 第一种
  console.log(localStorage.getItem(key));
  // 第二种
  console.log(localStorage[key]);
}
// 删除
function deleteLocalStorage(key) {
  // 两种方式删除 sessionStorage 中对应的数据
  // 第一种
  localStorage.removeItem(key);
  // 第二种
  delete localStorage[key];
}
```
需要注意的一点是 localStorage 和 sessionStorage 都只能存储字符串或者纯文本，所以如果想把 json 数据存进去的话，需要先序列化，获取的时候需要再转回来：
```javascript
const data = {"age": 21, "name": "tikitaka"};
// 设置
localStorage.setItem("user", JSON.stringify(data));
// 获取
const user = JSON.parse(localStorage.getItem("user"));
// ... 
```

### cookie 
cookie 与 localStorage 和 sessionStorage 还是有挺多区别的：   

1. 最大容量只有 4KB
2. 浏览器每次像服务端请求数据的时候，都会把 cookie 带过去给服务端。换句话说， cookie 在前端和服务端都可以访问得到，但是 localStorage 和 sessionStorage 只能在前端访问到
3. localStorage 和 sessionStorage 是 HTML5 的技术，之前的 html 版本不支持，cookie 支持 html4 和 html5
4. cookie 可以设置过期时间

一般来说，cookie 用作服务端判断多次请求是不是来自于一个客户端，可用于登录，或者购物车等其他常见场景。   
前端操作 cookie 可以通过 document.cookie API 设置，获取 cookie
```javascript
// 简单的设置 cookie
document.cookie = "AuthKey=weu5u34pongfer9tuewoiehjfi";
// 过期时间的设置
function setCookie() {
    const date = Date.now() + 24 * 7 * 3600000;
    const expires = new Date(date).toUTCString(); // Sat, 09 Nov 2019 10:07:42 GMT
    console.log(expires);
    // 7 天后过期
    document.cookie = `AuthKey=weu5u34pongfer9tuewoiehjfi;expires=${ expires }`
}
// 删除 cookie
// 只需要把过期时间设置靠前一点就可以了(可以设置一个已经过去的时间)
function deleteCookie() {
    const date = new Date().getTime() - 24 * 7 * 3600000;
    const expires = new Date(date).toUTCString();
    document.cookie = `AuthKey=weu5u34pongfer9tuewoiehjfi;expires=${ expires }`
}
```


相关链接：   
[HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)
[Web Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API)

