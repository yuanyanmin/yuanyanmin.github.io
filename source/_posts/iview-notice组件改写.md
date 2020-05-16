---
title: iview notice组件改写
date: 2020-05-16 16:55:13
tags: iview
categories:
toc:
---

前言：

在使用 iview 组件库时候，发现 notice 组件只能从上面弹出，这不能满足的项目需求，故对其进行修改

思路：

1. 多传入一个变量，代表弹出方向，将样式重写 top => bottom,
2. 当有多条数据时候，修改数据添加方式，从尾部改为头部 push => unshift
3. 在 index.js 引入组件时，自定义配置

<!--more-->

**源码目录结构**

* notice 的入口文件

![](/photo/iview/directory-1.jpg)

```
// 考虑到代码长度，故只放上部分涉及到修改的代码
// 定义变量，popUpDirection 传入弹出的方向，通过 config 传入配置
import Notification from '../base/notification';

let top = 24;
let defaultDuration = 4.5;
let noticeInstance;
let name = 1;

// Modify
let bottom = 24;
let maxHeight = '80%'

function getNoticeInstance (popUpDirection) {
  // Modify
  if (popUpDirection == 'top') {
        noticeInstance = noticeInstance || Notification.newInstance({
            prefixCls: prefixCls,
            styles: {
                top: `${top}px`,
                right: 0
            }
        });
    } else if (popUpDirection == 'bottom') {
        noticeInstance = noticeInstance || Notification.newInstance({
            prefixCls: prefixCls,
            styles: {
                bottom: `${bottom}px`,
                right: 0,
                maxHeight: `${maxHeight}`
            }
        });
    }

    return noticeInstance;
}

function notice (type, options) {
    const title = options.title || '';
    const desc = options.desc || '';
    const noticeKey = options.name || `${prefixKey}${name}`;
    const onClose = options.onClose || function () {};
    const render = options.render;
    // todo const btn = options.btn || null;
    const duration = (options.duration === 0) ? 0 : options.duration || defaultDuration;

    // Modify
    const popUpDirection = options.popUpDirection || 'top'



    name++;

    // Modify
    let instance = getNoticeInstance(popUpDirection);

    // Modify
    instance.notice({
      name: noticeKey.toString(),
      duration: duration,
      styles: {},
      transitionName: 'move-notice',
      content: content,
      withIcon: withIcon,
      render: render,
      hasTitle: !!title,
      onClose: onClose,
      closable: true,
      type: 'notice',
      popUpDirection: popUpDirection
    });
}

export default {
    open (options) {
        return notice('normal', options);
    },
    config (options) {
        if (options.top) {
            top = options.top;
        }
        if (options.duration || options.duration === 0) {
            defaultDuration = options.duration;
        },
        // Modify
        if (options.bottom) {
            bottom = options.bottom;
        }
        if (options.maxHeight) {
            maxHeight = options.maxHeight;
        }
    },
};
```

* notification 组件文件

![](/photo/iview/directory-2.jpg)

![](/photo/iview/directory-3.jpg)

```
// 通过自定义的 popUpDirection 判断数据插入位置，头或者尾部

// notification/index.js
notice (noticeProps) {
  notification.add(noticeProps)
},

// notification/notification.vue
 add (notice) {
  const name = notice.name || getUuid();
  let _notice = Object.assign({
    styles: {
      right: '50%'
    },
    content: '',
    duration: 1.5,
    closable: false,
    name: name
  }, notice);
  if (notice.popUpDirection == 'top') {
    this.notices.push(_notice);
  } else if (notice.popUpDirection == 'bottom') {
    this.notices.unshift(_notice)
  }
  this.tIndex = this.handleGetIndex();
},
```

**使用**

```
// vue 中 index.js

import Notice from './components/notice'

Vue.prototype.$Notice.config({
  bottom: 20,
  duration: 8,
  maxHeight: '60%' // 可传入百分比，或者 具体数字 + px
});

// test.vue
let i = 1;
setInterval(() => {
  // 关键代码
  this.$Notice.open({
     popUpDirection: 'bottom',
     title: i++,
     desc: 'This notification does not automatically close,and you need to click the close button to close.',
   })
}, 1000)

```

