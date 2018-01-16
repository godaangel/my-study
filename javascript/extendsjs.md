# 扩展的一些JS方法

> 主要是用原型链来扩展，也会包含其他扩展模式

#### 1、数组插入扩展

该方法主要是用于数组的插入扩展，方便直接写inster来插入，而不关注splice方法。

【splice传送门】[splice方法解析](http://www.w3school.com.cn/jsref/jsref_splice.asp "splice方法解析")

```js
Array.prototype.insert = function (index, item) {  
 this.splice(index, 0, item);  
};
```
