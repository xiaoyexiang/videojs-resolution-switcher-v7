# videojs-resolution-switcher-v7

 videojs@7 切换清晰度，代码来源于![videojs-resolution-switcher](https://github.com/kmoskwiak/videojs-resolution-switcher)，可是作者在videojs@5发布，该项目已经4年没维护了，之前很多人提了PR也没有合并，但是现在![video.js](https://github.com/videojs/video.js)都上@7了，直接引用原作者的代码库无法继续运作，所以在原代码的基础上做了一些 bugfix，还有一些优化，后面有时间会把我公司的 Demo ，也发出来。

> 特此声明：该包仅个人开发以及维护，后期不一定会维护，入坑慎重！如若发生版权问题，最终版权归原作者所有![kmoskwiak](https://github.com/kmoskwiak)，若使用该包所产生的法律责任，本人概不负责！

## 安装

```npm```安装```videojs-resolution-switcher-v7```插件

``` sh
npm i @xiaoyexiang/videojs-resolution-switcher-v7
```

## 引用、使用

``` html
<video id='video' class="video-js vjs-default-skin"></video>
<script src="video.js"></script>
<script src="videojs-resolution-switcher.js"></script>
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

## kmoskwiak/videojs-resolution-switcher

![kmoskwiak/videojs-resolution-switcher](https://github.com/kmoskwiak/videojs-resolution-switcher)
