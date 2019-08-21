# 小程序的视频内容流自动播放

> 啊啊啊，又解决一个问题

### 0、起因

这个需求产生的起因，是在做内容流（包含文本，图片，视频）的时候，需要如果流里面有视频，则滚动到一定位置时自动播放视频，类似朋友圈、微博等等的自动播放效果。



### 1、第一版尝试

第一版的思路是：

* 收集当前所有内容流相对于页面头部的高度，做成一个Array
* 滚动过程中，监听页面滚动事件，当达到某个高度要求，则播放对应的索引视频

这个操作缺点太多了，捡几个主要的说

缺点：

* 内容流是一个个的组件，获取距离顶部高度不方便，也不太准。并且组件内需要通过事件传播到列表页，在列表页进行高度Array整理、事件监听、切换索引等等（如果有几种列表页，就要写几遍，很麻烦）
* 监听滚动事件本身就消耗性能，做了节流也不是那么优秀



### 2、第二版尝试

突然，就发现了`wx.createIntersectionObserver`这个属性，它的作用是：返回`intersectionObserver`对象，用于推断某些节点是否可以被用户看见、有多大比例可以被用户看见（创建一个目标元素，根据目标元素和视窗的相交距离来判断当前页面滚动的情况。通常这个方案也用于页面图片的懒加载）。参考[https://developers.weixin.qq.com/miniprogram/dev/api/wxml/IntersectionObserver.html](https://developers.weixin.qq.com/miniprogram/dev/api/wxml/IntersectionObserver.html "微信小程序文档")

怎么解释呢，就是可以理解为，做一个监听，如果当前被监听的元素，进入了你规定的视界或者离开你规定的视界，就触发。

那么，怎么做到监听呢，参考如下代码：

```js
/** 监控视频是否需要播放 */
const videoObserve = wx.createIntersectionObserver()
videoObserve.relativeTo('.view-port')
    .observe(`#emotion${this.data.randomId}`, (res) => {
        let {intersectionRatio} = res
        if(intersectionRatio === 0) {
            //离开视界，因为视窗占比为0，停止播放
            this.setData({
                playstart: false
            })
        }else{
            //进入视界，开始播放
            this.setData({
                playstart: true
            })
        }
        
    })
```

其中，`observe` 是对应你需要监听的视频（也就是滚动进入视窗的元素）

那么，为什么选择relativeTo呢，是因为我们需要对它进入某一个视窗进行监听，而不是对进入整个屏幕视窗监听。

说了半天，还是没有说这个`.view-port`是啥，这里，假设我们让视频上半部分在触碰到屏幕中央时，就开始播放，视频下半部分在离开屏幕中央时，就停止播放，那么这个元素就很好理解了，如下：

```js
<view class="view-port" style="height: 100rpx; position: fixed; z-index: 1;width: 100%;letf:0;top:50%;transform: translateY(-50%);"></view>
```

让这个元素始终固定在屏幕中央，并且占有一定高度，假设为H，那么你滚动切换一个视频从播放到停止的距离是 videoHeight + H，这里自己体会一下，滚动距离不同而已，如果H过大，超过了两个视频之间的间隔，就会冲突，不建议过大。



PS：有个小地方，如果是在组件里面用，这个view-port建议写到组件里面，因为组件里监听是获取不到父页面的wxml节点的哦~相当于每个组件都有个这个view-port，不过不是很影响就是了~



代码片段稍后补上~

