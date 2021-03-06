---
description: 视频插件
---

# video

> 参考：[https://segmentfault.com/a/1190000018914486](https://segmentfault.com/a/1190000018914486)
>
>
>
> 最近的项目中需要播放视频，鉴于html5元素&lt;video&gt;的一些坑及不想自己造轮子，于是就找到了web端播放视频使用量最多的插件`video.js`，video.js是国外开发者开发的，英语本身就不好的我看英文文档简直是折磨，国内又没有中文文档，能搜的到的基本是简单的使用及最基本的api的介绍，想要实现一些自定义功能无从下手，所以我在这里整理一份我所遇到的问题及解决方法

### 1、视频初始化 <a id="item-1"></a>

video.js有两种初始化方式，一种是在video的html标签之中，一种是使用js来进行初始化

#### 1.1、在video中进行初始化 <a id="item-1-1"></a>

```jsx
<video
    id="my-player"
    class="video-js"
    controls
    preload="auto"
    poster="//vjs.zencdn.net/v/oceans.png"
    width="600"
    height="400"
    data-setup='{}'>
  <source src="//vjs.zencdn.net/v/oceans.mp4" type="video/mp4"></source>
  <source src="//vjs.zencdn.net/v/oceans.webm" type="video/webm"></source>
  <source src="//vjs.zencdn.net/v/oceans.ogv" type="video/ogg"></source>
  <p class="vjs-no-js">
    To view this video please enable JavaScript, and consider upgrading to a
    web browser that
    <a href="https://videojs.com/html5-video-support/" target="_blank">
      supports HTML5 video
    </a>
  </p>
</video>
```

效果  


![&#x56FE;&#x7247;&#x63CF;&#x8FF0;](../../../.gitbook/assets/3694922997-5cb8552816ca9_articlex.png)

#### 1.2、使用js进行初始化 <a id="item-1-2"></a>

```jsx
<!-- vjs-big-play-centered可使大的播放按钮居住，vjs-fluid可使视频占满容器 -->
<video id="myVideo" class="video-js vjs-big-play-centered vjs-fluid">
  <p class="vjs-no-js">
    To view this video please enable JavaScript, and consider upgrading to a
    web browser that
    <a href="https://videojs.com/html5-video-support/" target="_blank">
      supports HTML5 video
    </a>
  </p>
</video>

<script>
var player = videojs(document.getElementById('myVideo'), {
  controls: true, // 是否显示控制条
  poster: 'xxx', // 视频封面图地址
  preload: 'auto',
  autoplay: false,
  fluid: true, // 自适应宽高
  language: 'zh-CN', // 设置语言
  muted: false, // 是否静音
  inactivityTimeout: false,
  controlBar: { // 设置控制条组件
    /* 设置控制条里面组件的相关属性及显示与否
    'currentTimeDisplay':true,
    'timeDivider':true,
    'durationDisplay':true,
    'remainingTimeDisplay':false,
    volumePanel: {
      inline: false,
    }
    */
    /* 使用children的形式可以控制每一个控件的位置，以及显示与否 */
    children: [
      {name: 'playToggle'}, // 播放按钮
      {name: 'currentTimeDisplay'}, // 当前已播放时间
      {name: 'progressControl'}, // 播放进度条
      {name: 'durationDisplay'}, // 总时间
      { // 倍数播放
        name: 'playbackRateMenuButton',
        'playbackRates': [0.5, 1, 1.5, 2, 2.5]
      },
      {
        name: 'volumePanel', // 音量控制
        inline: false, // 不使用水平方式
      },
      {name: 'FullscreenToggle'} // 全屏
    ]
  },
  sources:[ // 视频源
      {
          src: '//vjs.zencdn.net/v/oceans.mp4',
          type: 'video/mp4',
          poster: '//vjs.zencdn.net/v/oceans.png'
      }
  ]
}, function (){
  console.log('视频可以播放了',this);
});
</script>
```

### 2、controlBar组件的说明 <a id="item-2"></a>

* `playToggle`, //播放暂停按钮
* `volumeMenuButton`,//音量控制
* `currentTimeDisplay`,//当前播放时间
* `timeDivider`, // '/' 分隔符
* `durationDisplay`, //总时间
* `progressControl`, //点播流时，播放进度条，seek控制
* `liveDisplay`, //直播流时，显示LIVE
* `remainingTimeDisplay`, //当前播放时间
* `playbackRateMenuButton`, //播放速率，当前只有html5模式下才支持设置播放速率
* `fullscreenToggle` //全屏控制

> `currentTimeDisplay`,`timeDivider`,`durationDisplay`是相对于 `remainingTimeDisplay`的另一套组件，后者只显示当前播放时间，前者还显示总时间。若要显示成前者这种模式，即 '当前时间/总时间'，可以在初始化播放器选项中配置：

```jsx
var myPlayer = neplayer('my-video', {controlBar:{
    'currentTimeDisplay':true,
    'timeDivider':true,
    'durationDisplay':true,
    'remainingTimeDisplay':false
}}, function() {
    console.log('播放器初始化完成');
});
```

![](../../../.gitbook/assets/3752102253-5cb85a71c5716_articlex.png)

### 3、video.js样式修改 <a id="item-3"></a>

```css
.video-js{ /* 给.video-js设置字体大小以统一各浏览器样式表现，因为video.js采用的是em单位 */
  font-size: 14px;
}
.video-js button{
  outline: none;
}
.video-js.vjs-fluid,
.video-js.vjs-16-9,
.video-js.vjs-4-3{ /* 视频占满容器高度 */
  height: 100%;
  background-color: #161616;
}
.vjs-poster{
  background-color: #161616;
}
.video-js .vjs-big-play-button{ /* 中间大的播放按钮 */
  font-size: 2.5em;
  line-height: 2.3em;
  height: 2.5em;
  width: 2.5em;
  -webkit-border-radius: 2.5em;
  -moz-border-radius: 2.5em;
  border-radius: 2.5em;
  background-color: rgba(115,133,159,.5);
  border-width: 0.12em;
  margin-top: -1.25em;
  margin-left: -1.75em;
}
.video-js.vjs-paused .vjs-big-play-button{ /* 视频暂停时显示播放按钮 */
  display: block;
}
.video-js.vjs-error .vjs-big-play-button{ /* 视频加载出错时隐藏播放按钮 */
  display: none;
}
.vjs-loading-spinner { /* 加载圆圈 */
  font-size: 2.5em;
  width: 2em;
  height: 2em;
  border-radius: 1em;
  margin-top: -1em;
  margin-left: -1.5em;
}
.video-js .vjs-control-bar{ /* 控制条默认显示 */
  display: flex;
}
.video-js .vjs-time-control{display:block;}
.video-js .vjs-remaining-time{display: none;}

.vjs-button > .vjs-icon-placeholder:before{ /* 控制条所有图标，图标字体大小最好使用px单位，如果使用em，各浏览器表现可能会不大一样 */
  font-size: 22px;
  line-height: 1.9;
}
.video-js .vjs-playback-rate .vjs-playback-rate-value{
  line-height: 2.4;
  font-size: 18px;
}
/* 进度条背景色 */
.video-js .vjs-play-progress{
  color: #ffb845;
  background-color: #ffb845;
}
.video-js .vjs-progress-control .vjs-mouse-display{
  background-color: #ffb845;
}
.vjs-mouse-display .vjs-time-tooltip{
  padding-bottom: 6px;
  background-color: #ffb845;
}
.video-js .vjs-play-progress .vjs-time-tooltip{
  display: none!important;
}
```

### 4、动态切换视频 <a id="item-4"></a>

```jsx
<script>
  var data = {
    src: 'xxx.mp4',
    type: 'video/mp4'
  };
  var player = videojs('myVideo', {...});
  player.pause();
  player.src(data);
  player.load(data);
  // 动态切换poster
  player.posterImage.setSrc('xxx.jpg');
  player.play();

  // 销毁videojs
  //player.dispose();
</script>
```

### 5、设置语言 <a id="item-5"></a>

#### 5.1传统形式开发 <a id="item-5-3"></a>

对于使用`<script>`标签形式的方式引入`video.js`，只需要在页面中引入你需要的语言包即可

```markup
<script src="//example.com/path/to/lang/es.js"></script>
<script src="//example.com/path/to/lang/zh-CN.js"></script>
<script src="//example.com/path/to/lang/zh-TW.js"></script>

<script>
var player = videojs('myVideo', {
    language: 'zh-CN' // 初始化时设置语言，立即生效
});

/* 动态切换语言
  使用这种方式进行动态切换不会立即生效，必须有所操作后才会生效。如播放按钮，必须点击一次播放按钮后播放按钮的提示文字才会改变  
 */
//player.language('zh-TW');
</script>
```

#### 5.2、vue开发 <a id="item-5-4"></a>

```jsx
import Video from 'video.js'
/* 不能直接引入js，否则会报错：videojs is not defined 
import 'video.js/dist/lang/zh-CN.js' */
import video_zhCN from 'video.js/dist/lang/zh-CN.json'
import video_en from  'video.js/dist/lang/en.json'
import 'video.js/dist/video-js.css'

Video.addLanguage('zh-CN', video_zhCN);
Video.addLanguage('en', video_en);
```

### 6、解决在iPhone中播放时自动全屏问题\(2019.09.23\) <a id="item-6"></a>

在iPhone设备上播放视频时\(微信浏览器上也会有这个问题\)会自动全屏，这里的全屏并不是常规的手机横屏那种全屏，而是类似于一个modal弹窗的全屏，解决办法就是在`video`标签中添加`playsinline="true"`属性

```markup
<video
    webkit-playsinline="true"
    playsinline="true"
    class="video-js vjs-big-play-centered vjs-fluid">
</video>
```

### 7、未解决的问题 <a id="item-7"></a>

控制条的高级自定义，如图中的进度条及时间在上面，播放按钮、上一个视频、下一个视频，设置及音量在下面这种控件该如何实现？

![&#x56FE;&#x7247;&#x63CF;&#x8FF0;](../../../.gitbook/assets/3246939790-5cb85d52398d2_articlex.png)

**如有知道实现这种高级自定义控制条方式的大神请在评论区留下您的代码**

### 8、参考文章 <a id="item-8"></a>

* [视频云web播放器样式和组件自定义](http://vcloud.163.com/vcloud-sdk-manual/WebDemos/LivePlayer_Web/introToComponent.html)
* [Video.js 踩坑简单入门](https://zhuanlan.zhihu.com/p/28338413)
* [免费视频播放器videojs中文教程](http://www.cnblogs.com/afrog/p/6689179.html)

