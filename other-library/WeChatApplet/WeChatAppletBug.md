# 微信小程序 100 个坑
> create by **jsliang** on **2018-9-17 17:58:56**  
> Recently revised in **2018-9-20 08:28:50**

&emsp;**是前端就填了这 100 个 bug 清单**！
  
&emsp;**如果你觉得本文还不错，记得给个 **star** ， 你的 **star** 是我学习的动力！**

&emsp;本文技术支持：**Ansen江**

<br>

## <span id="1">一、目录<span>

| 目录 | 坑数 |
| --- | --- |
| [一、前言](#1) | --- |
| [二、目录](#2) | --- |
| [三、内部组件坑](#3) | --- |
| [3.1 swiper 轮播图](#3-1) | 4 |
| [3.2 API: tabBar 与 switchTab](#3-2) | 1 |
| [四、开发自带坑](#4) | --- |
| [4.1 rpx](#4-1) | 1 |
| [4.2 开发工具](#4-2) | 1 |
| [4.3 组件与API](#4-3) | 1 |
| [4.4 flex布局](#4-4) | 2 |
| [4.5 无法设置 background-image](#4-5) | 1 |
| [4.6 \<block\> 与 \<view\>](#4-6) | 1 |
| [4.6 margin-top 无法上浮 ](#4-7) | 1 |

&emsp;目前已有 **13** 个坑。

<br>

## <span id="2">二、前言</span>

&emsp;[返回目录](#1)

&emsp;微信小程序的教程，或许写出来是非常受欢迎的。但是，第一微信小程序是国内的，有 [中文文档](https://developers.weixin.qq.com/miniprogram/dev/index.html) （虽然它文档的说明有点坑）。第二，想想我带你们走了一条平坦路，之后你们碰到坑坑洼洼了，咋办？最后的锅，会不会到我背啊，可怕！  
&emsp;所以，在这里， jsliang 结合 **“日常躺坑”** ，先为你解决小程序的 100 个坑！虽然现在可能还不够，但是第一天我就碰到 4/5 个了，我想我可以帮你们躺完 100 个的！！！  
&emsp;现在的微信开发者工具显示的开发版本是：`"libVersion": "2.0.4"`  
&emsp;如果你开发的版本已经解决了这个 bug ，或者你觉得这个 bug 还有其他解决方法，或者你觉得这个玩意还有其他 bug ，请告诉我！  
&emsp;QQ： 1741020489  
&emsp;后来者开心无坑开发日，天祭无忘告 jsliang ！跪谢！！！

<br>

## <span id="3">三、内部组件坑</span>

&emsp;[返回目录](#1)

> 本坑来源于微信自带的开发文档：[小程序开发](https://developers.weixin.qq.com/miniprogram/dev/component/)

<br>

### <span id="3-1">3.1 swiper 轮播图</span>

&emsp;[返回目录](#1)

> 本组件目前已有4个坑，有兴趣的小伙伴可以详看。

摘选自该地址：[地址](https://developers.weixin.qq.com/miniprogram/dev/component/swiper.html)。下面是原版代码：
> demo.wxml
```
<swiper indicator-dots="{{indicatorDots}}"
  autoplay="{{autoplay}}" interval="{{interval}}" duration="{{duration}}">
  <block wx:for="{{imgUrls}}">
    <swiper-item>
      <image src="{{item}}" class="slide-image" width="355" height="150"/>
    </swiper-item>
  </block>
</swiper>
<button bindtap="changeIndicatorDots"> indicator-dots </button>
<button bindtap="changeAutoplay"> autoplay </button>
<slider bindchange="intervalChange" show-value min="500" max="2000"/> interval
<slider bindchange="durationChange" show-value min="1000" max="10000"/> duration
```
> demo.js
```
Page({
  data: {
    imgUrls: [
      'http://img02.tooopen.com/images/20150928/tooopen_sy_143912755726.jpg',
      'http://img06.tooopen.com/images/20160818/tooopen_sy_175866434296.jpg',
      'http://img06.tooopen.com/images/20160818/tooopen_sy_175833047715.jpg'
    ],
    indicatorDots: false,
    autoplay: false,
    interval: 5000,
    duration: 1000
  },
  changeIndicatorDots: function(e) {
    this.setData({
      indicatorDots: !this.data.indicatorDots
    })
  },
  changeAutoplay: function(e) {
    this.setData({
      autoplay: !this.data.autoplay
    })
  },
  intervalChange: function(e) {
    this.setData({
      interval: e.detail.value
    })
  },
  durationChange: function(e) {
    this.setData({
      duration: e.detail.value
    })
  }
})
```
<br>

&emsp;好的，上面就是微信官方文档的演示代码，那么我们开始填坑：
1. `demo.wxml` 中出现的 `<image src="{{item}}" class="slide-image" width="355" height="150"/>` 这行， `width` 和 `height` 的行内属性是忽悠老百姓的，**它并没卵用** ！我们需要在 `slide-image` 这个 `class` 类中修改 `width` 和 `height`。
2. 绑定值。官网内容：
![图](../../public-repertory/img/other-WechatApplet-bug-1.png)
&emsp;它的属性名和属性值是这么说的，然后，用的时候，首先你需要在 `demo.wxml` 中的 `swiper` 绑定这个属性名，然后在 `demo.js` 中设置其属性值。值得注意的是，它的绑定值，稍微不同于 `Vue`， 需要设置 `{{}}` 形式。
3. 关于轮播图的地址跳转，在微信小程序的官网是没用提及的，也是 jsliang 去百度查看了下，才知道怎么设置（可能是我一开始就挑战的难度太高了么-_-||），在下面 jsliang 贴出来代码~想知道怎么解决的可以去看看：首先，在 `data` 中设置 `link` ；然后，设置 `navigator` 导航遍历 `item.link` 。
4. 关于 `wx:key` ， `wx:key` 的作用是：当数据改变触发渲染层重新渲染的时候，会校正带有 key 的组件，框架会确保他们被重新排序，而不是重新创建，以确保使组件保持自身的状态，并且提高列表渲染时的效率。但是，在其 `swiper` 中，小程序本身是没有写的，所以它会带有 `warning` ，这里也是个小坑， jsliang 也是百度了下也知道这件事。[点我了解](https://www.sohu.com/a/207728111_99897596)

<br>

&emsp;下面给出 jsliang 的解决代码，如果不对，尽情指出：
> index.wxml
```
<view>
  <swiper indicator-dots="{{indicatorDots}}" autoplay="{{autoplay}}" interval="{{interval}}" duration="{{duration}}" indicator-color="#707071" indicator-active-color="#fff" circular="true">
    <!-- wx:key ： 提高列表渲染效率 -->
    <block wx:for="{{carousel}}" wx:key="{{item.index}}">
      <swiper-item>
        <navigator url="{{item.link}}" hover-class="navigator-hover">
          <image src="{{item.url}}" class="slide-image" />
        </navigator>
      </swiper-item>
    </block>
  </swiper>
</view>
```

> index.wxss
```
.slide-image {
  width: 100%;
  height: 420rpx;
}
```

> index.js
```
Page({
  data: {
    imgUrls: [
      {
        link: '../index/index',
        url: 'http://img02.tooopen.com/images/20150928/tooopen_sy_143912755726.jpg',
      },
      {
        link: '../demo/demo',
        url: 'http://img06.tooopen.com/images/20160818/tooopen_sy_175866434296.jpg',
      },
      {
        link: '../logs/logs',
        url: 'http://img02.tooopen.com/images/20150928/tooopen_sy_143912755726.jpg'
      }
    ],
    indicatorDots: true,
    autoplay: true,
    interval: 5000,
    duration: 1000
  }
})
```

<br>

### 3.2 <span id="3-2">API: tabBar 与 switchTab</span>

&emsp;[返回目录](#1)

> 本组件目前已有1个坑，有兴趣的小伙伴可以详看。

&emsp;tabBar ：底部菜单栏，需要在 `app.json` 中设置。使用方法：见下文。  
&emsp;navigator ：导航切换。[使用方法](https://blog.csdn.net/u013778905/article/details/59141486)  
&emsp;switchTab ：控制 tabBar 的切换。[使用方法](https://blog.csdn.net/liona_koukou/article/details/53930045)

&emsp;在这里，我们讲下 `tabBar` 的坑，如果你在 `app.json` 中设置了 `tabBar` ：
> app.json
```
"tabBar": {
    "list": [{
      "pagePath": "pages/index/index",
      "text": "首页",
      "iconPath": "./public/index_tabBar1_nor.png",
      "selectedIconPath": "./public/index_tabBar1.png"
    }, {
      "pagePath": "pages/demo/demo",
      "text": "发现",
      "iconPath": "./public/index_tabBar2_nor.png",
      "selectedIconPath": "./public/index_tabBar2.png"
    }, {
      "pagePath": "pages/logs/logs",
      "text": "我的",
      "iconPath": "./public/index_tabBar3_nor.png",
      "selectedIconPath": "./public/index_tabBar3.png"
    }]
  }
```

&emsp;那么，我们就需要通过设置 `switchTab` 来控制底部导航的跳转，而不能通过 `navigator` 来跳转：
> demo.wxml
```
<view>
  <button bindtap="linkTo">Hello</button>
</view>
```
> demo.js
```
linkTo: function () {
  wx.switchTab({
    url: '../index/index'
  });
},
```

<br>

## <span id="4">四、开发自带坑</span>

<br>

&emsp;[返回目录](#1)

> 本坑来源于开发中产品提出的需求，代码开发中碰到的坑 。

<br>

### <span id="4-1">4.1 rpx</span>

&emsp;[返回目录](#1)

> 本节目前已有1个坑，有兴趣的小伙伴可以详看。

&emsp;在微信中，它自带了一套属于自己的单位：`rpx` ， `rpx` 不同于之前我们认识的 `px` 、 `rem` 、 `em` ，如果你的设计稿是 750 px 的，那么很容易的， 1px = 1rpx ，但是，如果设计稿不是 750 px ，那么将造成一个 bug ，至于这个 bug 如何解决……谁知道呢……  
&emsp;知识补充：[关于 rpx](http://www.51xuediannao.com/javascript/xiaochengxu_rpx.html) 。

<br>

### <span id="4-2">4.2 开发工具</span>

&emsp;[返回目录](#1)

> 本节目前已有1个坑，有兴趣的小伙伴可以详看。

1. bug：开发工具无法输入中文。  
解决方式：重启开发工具。

<br>

### <span id="4-3">4.3 组件与API</span>

&emsp;[返回目录](#1)

> 本节目前已有1个坑，有兴趣的小伙伴可以详看。

&emsp;在微信小程序官方文档中，组件与API，是拆分的。是的，轮播图与底部导航条，一个在组件中，一个在导航条中；一个在 `wxml` 、 `wxss` 、 `js` 中要设置对应的参数，一个只需要在 `app.json` 中设置就行，可能微信小程序考虑到底部导航条不应该有太大的变化（例如让你修改样式），所以将导航条内嵌至源码中了。

<br>

### <span id="4-4">4.4 flex布局</span>

&emsp;[返回目录](#1)

> 本节目前已有2个坑，有兴趣的小伙伴可以详看。

&emsp;Flex布局又称弹性布局，在小程序开发中比较适用。但是由于 jsliang 之前没怎么用过 Flex 布局，所以在这里咱们特意去踩下坑，充实下自己。[链接1](https://blog.csdn.net/anda0109/article/details/72867449) [链接2](http://www.techug.com/post/flex-examples.html)

1. 左右布局
> 图待上传

&emsp;如图，这是我们要实现的左右布局效果。那么，在微信小程序要怎么做呢？
> 代码待上传

2. 混合布局
> 图待上传

&emsp;如图，这是我们要实现的左右布局效果。那么，在微信小程序要怎么做呢？
> 代码待上传

<br>

### <span id="4-5">4.5 无法设置 background-image</span>

&emsp;[返回目录](#1)

> 本节目前已有1个坑，有兴趣的小伙伴可以详看。

&emsp;在小程序中，如果你使用 wxss，你是可以发现有 `background-image` 的提示的。但是，如果你设置它的背景图是本地图片，你会发现，它是不生效的。  
&emsp;解决方案：  

1. 在使用背景图片的时候用网络图片，就是用外链的形式，比如你将这张图片放到你的服务器，如：`https://xxxx/xxx.jpg`；
2. 将背景图片使用编码base64进行转换，可以在这个网址进行 [点我前往](http://tool.css-js.com/base64.html) 转换，如：background-image: url("转换后得到的编码文本")，如果多次使用的话可以将该值设置为全局变量，再在js文件进行引用即可。
3. 使用 `image` 组件 + `position` 定位而不是使用 `background-image` 。

<br>

### <span id="4-6">4.6 `<block>` 与 `<view>`</span>

&emsp;[返回目录](#1)

> 本节目前已有1个坑，有兴趣的小伙伴可以详看。

&emsp;两者的区别是，`<view>` 是一个组件，会在页面上做渲染；`<block>` 不是一个组件，它仅仅是一个包装元素，只接受控制属性，不会在页面中做任何渲染。  
&emsp;所以，如果你仅仅是需要包裹，而不是渲染一个层，可以使用 `<block>` 提升性能。

<br>

### <span id="4-7">4.7 margin-top 无法上浮 

&emsp;[返回目录](#1)

> 本节目前已有1个坑，有兴趣的小伙伴可以详看。

&emsp;首先，我们要实现的效果是：

![图](../../public-repertory/img/other-WechatApplet-bug-2.png)

&emsp;然后， jsliang 的想法是：

> *.wxml
```
<view class="search">
  <input class="search-product-input" bindinput="bindKeyInput" auto-focus maxlength='10'></input>
  <label class="search-placeholder">
    <image class="search-placeholder-icon" src="../../public/index_indexProduct_icon_search.png"></image>
    <text class="search-placeholder-text">搜索产品</text>
  </label>
  <view></view>
</view>
```

<br>

> *.wxss
```
.search {
  height: 100rpx;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  position: relative;
}
.search-product-input {
  background: #f5f5f5;
  width: 650rpx;
  height: 65rpx;
  border-radius: 30rpx;
  font-size: 30rpx;
  padding-left: 20rpx;
}
.search-placeholder {
  font-size: 26rpx;
  text-align: center;
  margin-top: -65rpx;
  z-index: 2;
}
.search-placeholder-icon {
  width: 24rpx;
  height: 24rpx;
}
.search-placeholder-text {
  margin-left: 10rpx;
}
```

<br>

&emsp;你注意到了吗？在 `*.wxml` 中， jsliang 设置了个空的 `<view>` ，如果你把这个 `<view>` 去掉，你会惊奇地发现，它……下来了……

![图](../../public-repertory/img/other-WechatApplet-bug-3.png)

&emsp;好吧，可能有其他的实现方式，但是如果你下次使用这种方式，注意这个坑~

<br>

> <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">jsliang的文档库</span> 由 <a xmlns:cc="http://creativecommons.org/ns#" href="https://github.com/LiangJunrong/document-library" property="cc:attributionName" rel="cc:attributionURL">梁峻荣</a> 采用 <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">知识共享 署名-非商业性使用-相同方式共享 4.0 国际 许可协议</a>进行许可。<br />基于<a xmlns:dct="http://purl.org/dc/terms/" href="https://github.com/LiangJunrong/document-library" rel="dct:source">https://github.com/LiangJunrong/document-library</a>上的作品创作。<br />本许可协议授权之外的使用权限可以从 <a xmlns:cc="http://creativecommons.org/ns#" href="https://creativecommons.org/licenses/by-nc-sa/2.5/cn/" rel="cc:morePermissions">https://creativecommons.org/licenses/by-nc-sa/2.5/cn/</a> 处获得。