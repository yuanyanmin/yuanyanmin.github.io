---
title: picture-in-picture
date: 2020-05-10 16:04:00
tags: 画中画
categories:
toc:
---

前言：

项目需求，为视频添加画中画功能，开始折腾，期间踩了一些坑，记录。

在b站、斗鱼直播、now直播等各大平台均有画中画这一功能的实现。

<!--more-->

### 进入/退出画中画模式

```
<video id="video" src="http://example.com/file.mp4"></video>

<button id="togglePipButton"></button>

<script>
    const video = document.getElementById('video');
    const togglePipButton = document.getElementById('togglePipbutton');

    // 当不支持画中画时隐藏该按钮
    togglePipButton.hidden = !document.pictureInPictureEnabled || video.disablePictureInPicture;

    togglePipButton.addEventListener('click', function() {
        // 如果没有画中画元素，则创建，否则，退出。
        if (!document.pictureInPictureElement) {
            video.requestPictureInPicture().catch(error => {
                // 进入画中画模式失败
            })
        } else {
            document.exitPictureInPicture().catch(error => {
                // 退出画中画模式失败
            })
        }
    })
</script>
```
### 监听画中画大小改变

```
<script>
    const video = document.getElementById('video')

    video.addEventListener('enterpictureinpicture', function(event) {
        // 进入画中画模式
        const pipWindow = event.pictureInPictureWindow;
        console.log('pip window width: ' + pipWindow.width);
        console.log('pip window height: ' + pipWindow.height);
    })

    video.addEventListener('leavepictureinpicture', function() {
        // 退出画中画模式
    })
</script>
```

### 更新视频大小当画中画窗口大小改变

```
let pipWindow;
const video = document.getElementById('video');
const pipButton = document.getElementById('pipButton');

pipButton.addEventListener('click', function() {
    video.requestPictureInPicture().catch(error => {
        
    })
})

video.addEventListener('enterpictureinpicture', function(event) {
    pipWindow = event.pictureInPictureWindow;
    updateVideoSize(pipWindow.width, pipWindow.height);
    pipWindow.addEventListener('resize', onPipWindowResize);
})

video.addEventListener('leavepictureinpicture', function() {
    pipWindow.removeEventListener('resize', onPipWindowResize);
});

function onPipWindowResize(event) {
    updateVideoSize(event.target.width, event.target.height)
}

function updateVideoSize(width, height) {
    // 根据pip窗口大小更改视频大小
}
```

### 自定义画中画窗口控件

默认情况下，浏览器在画中画窗口中显示 “播放/暂停” 按钮，除非视频正在播放 MediaStream 对象（由虚拟视频源，eg. 相机、视频记录设备、屏幕共享服务或其他硬件产生来源）

*记录：在做视频直播画中画时候，因为测试环境没有直播服务不能用，故用的本地视频，导致暂停按钮一直存在，在看了其他直播网站的画中画（斗鱼，Now直播）一直坚信有什么属性可以控制显示，最后看到这句话，换到其他环境测试，没有暂停按钮，crying...*

```
navigator.mediaSession.setActionHandle('previousstrack', () => {
    // 转到上一个控件
})

navigator.mediaSession.setActionHandle('nexttrack', () => {
    // 转到下一个控件
})
```

### 补充

* 只能有一个画中画窗口
* 回到标签页/关闭画中画窗口时候，原视频会暂停，如需要继续播放，需要手动更新视频源
* Auto Picture-in-picture 自动画中画（功能很强大，支持性不高）
* 使用 MediaSession 可以自定义窗口的控件

**参考链接**

[掘金](https://juejin.im/post/5d1da283e51d45106343187b)
[知乎](https://zhuanlan.zhihu.com/p/38251413)
[demo](https://googlechrome.github.io/samples/picture-in-picture/)
[w3c](https://w3c.github.io/picture-in-picture/)
[MediaSession介绍](https://web.dev/media-session/)
[其他](https://css-tricks.com/an-introduction-to-the-picture-in-picture-web-api/)