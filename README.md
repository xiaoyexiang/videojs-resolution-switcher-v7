# videojs-resolution-switcher-v7

 videojs@7 切换清晰度，代码来源于[videojs-resolution-switcher](https://github.com/kmoskwiak/videojs-resolution-switcher)，可是作者在videojs@5发布，该项目已经4年没维护了，之前很多人提了PR也没有合并，但是现在[video.js](https://github.com/videojs/video.js)都上@7了，直接引用原作者的代码库无法继续运作，所以在原代码的基础上做了一些 bugfix，还有一些优化，后面有时间会把我公司的 Demo ，也发出来。

> 特此声明：该包仅个人开发以及维护，后期不一定会维护，入坑慎重！如若发生版权问题，最终版权归原作者所有[kmoskwiak](https://github.com/kmoskwiak)，若使用该包所产生的法律责任，本人概不负责！

## GitHub

各位喜欢的不妨到本项目 [GitHub](https://github.com/xiaoyexiang/videojs-resolution-switcher-v7) 的给个start，谢谢

## 安装

```npm```安装```videojs-resolution-switcher-v7```插件

``` sh
npm i @xiaoyexiang/videojs-resolution-switcher-v7
```

## 引用、使用

记得引用 css 文件，如果样式不是你想要的，可以自己定义 css

### 普通引用

``` html
<link rel="stylesheet" href="videojs.css">
<link rel="stylesheet" href="videojs-resolution-switcher-v7.css">
<video id='video' class="video-js vjs-default-skin"></video>
<script src="video.js"></script>
<script src="videojs-resolution-switcher-v7.js"></script>
<script>
  videojs('video', {
    controls: true,
    plugins: {
        videoJsResolutionSwitcher: {
          default: 'high',
          dynamicLabel: true
        }
      }
  }, function(){
  
    // Add dynamically sources via updateSrc method
    player.updateSrc([
        {
          src: 'http://media.xiph.org/mango/tears_of_steel_1080p.webm',
          type: 'video/webm',
          label: '360'
        },
        {
          src: 'http://mirrorblender.top-ix.org/movies/sintel-1024-surround.mp4',
          type: 'video/mp4',
          label: '720'
        }
      ])

      player.on('resolutionchange', function(){
        console.info('Source changed to %s', player.src())
      })
  })
</script>
```

### Nuxt.js 使用

本人项目是使用 Nuxt.js 的，下面贴上我的代码

``` vue
<template>
    <div ref="playerWrap" class="player-wrap">
        <video
        id="elm-yjPlayer"
        ref="yjPlayer"
        class="player video-js vjs-theme-fantasy vjs-default-skin"
        ></video>
    </div>
</template>

<script>
import 'video.js/dist/video-js.min.css'
import '@xiaoyexiang/videojs-resolution-switcher-v7/lib/videojs-resolution-switcher-v7.css'
import '~/assets/css/videojs-themes/fantasy/index.css'
import videojs from 'video.js'

// 因为项目内的 js 使用了 window，所以必须要在客户端 ```process.client``` 引入，不然打包的时候会报错
if (process.client) {
  window.videojs = videojs
  require('video.js/dist/lang/zh-CN.js')
  require('@xiaoyexiang/videojs-resolution-switcher-v7')
}

export default {
  data() {
    return {
      playOptions: {
        autoplay: false,
        controls: true,
        playbackRates: [0.75, 1, 1.5, 2],
        language: 'zh-CN',
        label: '超清',
        plugins: {
          videoJsResolutionSwitcher: {
            dynamicLabel: true,
            default: 'high'
          }
        },
        sources: [],
        controlBar: {
          pictureInPictureToggle: false
          // fullscreenToggle: false
        },
        html5: {
          hls: {}
        },
        userActions: {
          hotkeys: true
        }
      }
    }
  },
  async asyncData({ $axios }) {
    let videoUrlsData = await $axios.get('/api/video_url')
      .then((res) => {
        return Promise.resolve(res.data)
      })
    /*
    * videoUrlsData 的格式
    * [
        {
          src: "https://1252685978.vod2.myqcloud.com/9a27125cvodtransgzp1252685978/c0155b3f5285890788786290644/v.f240.m3u8?t=5e574d42&rlimit=5&us=e163173c30&sign=3ccd11cef7ee232ecabc2b0676694d45"
          type: "application/x-mpegURL"
          label: "超清"
          res: 1080
        },
        {
          src: "https://1252685978.vod2.myqcloud.com/9a27125cvodtransgzp1252685978/c0155b3f5285890788786290644/v.f230.m3u8?t=5e574d42&rlimit=5&us=d2c9b54554&sign=8f092e038987bfc506f43f3d8249e3ba"
          type: "application/x-mpegURL"
          label: "高清"
          res: 720
        },
        {
          src: "https://1252685978.vod2.myqcloud.com/9a27125cvodtransgzp1252685978/c0155b3f5285890788786290644/v.f220.m3u8?t=5e574d42&rlimit=5&us=a72989e3b8&sign=7201dc0ce1f68778fd4b6844f34b17ab"
          type: "application/x-mpegURL"
          label: "标清"
          res: 480
        }
      ]

      后台接口返回的数据格式可能不是我们想要的，可以使用 js 遍历处理下，让格式化成为上方例子
    */
    return {
      videoUrlsData
    }
  },
  mounted() {
    this.initVideoPlayer()
  },
  beforeDestroy() {
    // 页面销毁的时候，也要销毁播放器实例
    if (this.player) {
      this.player.dispose()
    }
  },
  methods: {
    initVideoPlayer() {
      this.playOptions.sources = this.videoSourceList
      if (process.client) {
        // eslint-disable-next-line
        this.player = videojs(this.$refs.yjPlayer, this.playOptions, () => {
          // console.log(this.playOptions)
          // console.log(this)
          // console.log(this.player)
          this.player.on('resolutionchange', () => {
            console.log('[切换视频源]')
          })
        })
      }
    },
  }
}
</script>

```

## kmoskwiak/videojs-resolution-switcher

[kmoskwiak/videojs-resolution-switcher](https://github.com/kmoskwiak/videojs-resolution-switcher)
