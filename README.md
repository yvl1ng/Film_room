# [电影屋] 使用WebSocket实现异地同步看电影



<meta name="referrer" content="no-referrer">



## 说明

放假回家之后，因为和女朋友不在同一个地方，又想着一起看电影，但市面上能够远程一起看电影的方法都无法满足现有的需求：

- 方案一：使用“微光”等app，虽然能够同步播放和实时聊天，但片源太少，想要上传还要认证，pass掉。
- 方案二：使用“腾讯会议”等软件，共享屏幕，这类方式对带宽有一定要求，网络延迟比较明显，体验不好，pass掉。

几番纠结，才想起来我就是学计算机的啊，有需求Coding就完了，于是折腾了一晚上，写出了[电影屋]这个小Demo。

目前已经实现的功能有：

- 远程同步播放视频
- 与腾讯云COS配合，播放任意视频（目前只支持mp4格式）
- 房间验证，使用房间ID和房间密码才能进入对应房间
- 多房间模式，使用唯一ID进行区分，房间之间互不干扰

仍待实现的功能：

- 调用腾讯云COS的api实现一键上传

- 增加实时聊天功能

- 增加弹幕功能

  ……



## 实现思路

这里偷了个懒，省去了文件存储和文件解析的步骤，直接采用了在对象存储（COS）上存储视频文件，通过服务器将视频外链分发给进入房间的用户，这样每位用户都是直接访问对象存储上的文件，而不消耗服务器流量。服务器只负责创建房间、房间验证、信息分发和进度同步的功能。

技术栈：

- 前端：Html + Css + Js
- 后端：Java + Websocket

同步方式：

建立WebSocket通信之后，加入心跳机制防止断开。同步信息以房间创建者为准，每五分钟房间创建者的浏览器向服务器发送一次当前播放信息，服务器将接收到的播放信息广播给其它用户，其它用户接收到广播信息，对内容进行校验，校验通过后同步本地信息。当任意用户暂停/播放视频时，将事件发送到服务器进行广播，其它用户根据广播内容相应地做出动作。



## 效果图

![1](https://yvling-typora-image-1257337367.cos.ap-nanjing.myqcloud.com/typora/1.png)











