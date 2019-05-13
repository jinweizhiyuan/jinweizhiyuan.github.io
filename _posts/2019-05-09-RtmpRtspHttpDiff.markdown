---
layout: post
title: RTSP、RTMP和HTTP协议的区别
date: 2019-05-09 14:27:55 +UTC800
catetories: [video]
tags: [video]
---

RTSP和RTMP是2个不同的网络传输协议，RTSP（Real Time Streaming Protocol），RFC2326，实时流传输协议,而RTMP是Real Time Messaging Protocol（实时消息传输协议），网络摄像机的厂家，可根据自身的需求，采用不同协议来处理。目前多数网络摄像机支持RTSP、ONVIF等通用的协议，而支持RTMP比较少。

RTMP是Real Time Messaging Protocol(实时消息传输协议)的首字母缩写。该协议基于TCP，是一个协议族，包括RTMP基本协议及RTMPT/RTMPS/RTMPE等多种变种。RTMP是一种设计用来进行实时数据通信的网络协议，主要用来在Flash/AIR平台和支持RTMP协议的流媒体/交互服务器之间进行音视频和数据通信。支持该协议的软件包括Adobe Media Server/Ultrant Media Server/red5等。

RTSP(Real Time Streaming Protocol)，RFC2326，实时流传输协议，是TCP/IP协议体系中的一个应用层协议，由哥伦比亚大学、网景和RealNetworks公司提交的IETF RFC标准。该协议定义了一对多应用程序如何有效地通过IP网络传送多媒体数据。RTSP在体系结构上位于RTP和RTCP之上，它使用TCP或UDP完成数据传输。HTTP与RTSP相比，HTTP请求由客户机发出，服务器作出响应;使用RTSP时，客户机和服务器都可以发出请求，即RTSP可以是双向的。RTSP是用来控制声音或影像的多媒体串流协议，并允许同时多个串流需求控制，传输时所用的网络通讯协定并不在其定义的范围内，服务器端可以自行选择使用TCP或UDP来传送串流内容，它的语法和运作跟HTTP 1.1类似，但并不特别强调时间同步，所以比较能容忍网络延迟。而前面提到的允许同时多个串流需求控制(Multicast)，除了可以降低服务器端的网络用量，更进而支持多方视讯会议(Video Conference)。因为与HTTP1.1的运作方式相似，所以代理服务器〈Proxy〉的快取功能〈Cache〉也同样适用于RTSP，并因RTSP具有重新导向功能，可视实际负载情况来转换提供服务的服务器，以避免过大的负载集中于同一服务器而造成延迟。

RTSP RTMPHTTP都是可以做视频直播或者点播，但一般做直播用RTSP RTMP，做点播用HTTP。做视频会议的时候原来用SIP协议，现在基本上被RTMP协议取代了，下面我们就来看看他们的作用到底有何不同。

__共同点：__

1. RTSP RTMP HTTP都是在应用应用层。
2. 理论上RTSP RTMPHTTP都可以做直播和点播，但一般做直播用RTSP RTMP，做点播用HTTP。做视频会议的时候原来用SIP协议，现在基本上被RTMP协议取代了

![示意](/assets/images/20190509001.jpg){:width="100%"}
121
**区别：**
1. HTTP: 即超文本传送协议(ftp即文件传输协议)。
      HTTP:（Real Time Streaming Protocol），实时流传输协议。
      HTTP全称Routing Table Maintenance Protocol（路由选择表维护协议）。
 
2. HTTP将所有的数据作为文件做处理。http协议不是流媒体协议。
      RTMP和RTSP协议是流媒体协议。
 
3. RTMP协议是Adobe的私有协议,未完全公开，RTSP协议和HTTP协议是共有协议，并有专门机构做维护。
 
4. RTMP协议一般传输的是flv，f4v格式流，RTSP协议一般传输的是ts,mp4格式的流。HTTP没有特定的流。
 
5. RTSP传输一般需要2-3个通道，命令和数据通道分离，HTTP和RTMP一般在TCP一个通道上传输命令和数据。
RTSP实时流协议 作为一个应用层协议，RTSP提供了一个可供扩展的框架，它的意义在于使得实时流媒体数据的受控和点播变得可能。总的说来，RTSP是一个流媒体表示 协议，主要用来控制具有实时特性的数据发送，但它本身并不传输数据，而是必须依赖于下层传输协议所提供的某些服务。RTSP可以对流媒体提供诸如播放、暂 停、快进等操作，它负责定义具体的控制消息、操作方法、状态码等，此外还描述了与RTP间的交互操作（RFC2326）。)


*引用<https://blog.csdn.net/liujiayu2/article/details/80658395>*