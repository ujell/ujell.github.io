---
title: SimpleWebRTC and iOS
date: 2016-10-09 09:00:00 +0200
categories: [iOS, WebRTC]
tags: [WebRTC, iOS, OpenWebRTC]   
---

In my current project, I need to create a video conferencing system. Since I have neither time nor any good reason to write it from the stretch, I went with WebRTC, specifically [SimpleWebRTC](https://simplewebrtc.com). These are basically my impressions and experience from 0 to working prototype.

# OpenWebRTC and AppRTC 

There are many projects on WebRTC. I first checked [OpenWebRTC](https://www.openwebrtc.org), but had major problems with the examples -they were simply not working and fixing the errors were taking long time (While writing this post, I realized they forgot to update `.podfile` with the current SDK, though I can't try it out since currently Ericsson Research servers are [down](https://github.com/EricssonResearch/openwebrtc-ios-sdk/issues/78)). This led me to [AppRTC](https://github.com/webrtc/apprtc), but I need to run server on the local machine while testing iOS and my Macbook Air is under-powered for that task. At this point I found SimpleWebRTC.

# SimpleWebRTC

WebRTC alone is responsible with RTC between peers. Connection management, like initiating or finishing the connection, is done by a separate signaling server -which should be provided by the developer. If you check [getting started code lab](https://codelabs.developers.google.com/codelabs/webrtc-web/#0) from Google, you can see how to create a signaling server alongside with WebRTC. This is a really good example to learn how WebRTC works, but it's also a bit complicated in the beginning and prone to error. That's where SimpleWebRTC comes into play.

SimpleWebRTC provides extremely simple programming interface and hides details from the developer. Moreover, it has an open source [signaling server](https://github.com/andyet/signalmaster) and [iOS implementation](https://github.com/otalk/TLKSimpleWebRTC).

I started with a simple page that connects to the demo server, but I stuck because of a very small and unexpected detail. For some reason, the most recent SimpleWebRTC script on their server is named "[latest-v2.js](https://simplewebrtc.com/latest-v2.js)", but "[latest.js](https://simplewebrtc.com/latest.js)" also exists. Unsurprisingly, `latest.js` is not working with the current version and I tried to solve the errors till I realized my mistake.

Besides that, creating a simple web page and running signalmaster locally is extremely simple and can be done in very short time with following instructions.

# SimpleWebRTC on iOS

As I mentioned earlier, SimpleWebRTC also has an iOS implementation and a [demo](https://github.com/otalk/iOS-demo). When I  tried, demo project run and connected to demo server without a problem. In the next step I tried to connect it to my local server.

Unfortunately, it didn't work. With some debugging, I realized Socket.IO versions of signalmaster (v1.3.7) and the library used to manage Socket.IO connection on iOS, [AZSocketIO](https://github.com/lukabernardi/AZSocketIO), (v0.9) were incompatible.

I couldn't find any simple workaround, except using an older version of signalmaster, therefore I've done the next reasonable thing; I removed AZSocketIO and replaced it with the official [Socket.IO iOS client](https://github.com/socketio/socket.io-client-swift).

## Sharing the new version

Naturally, I wanted to share this on GitHub and make it usable with CocoaPods. This didn't go as I wanted...

TLKSimpleWebRTC depends on [TLKWebRTC](https://github.com/otalk/TLKWebRTC), which depends on Google's WebRTC library for iOS. This library updated a lot and normally developers should compile and add it to their project [themselves](https://webrtc.org/native-code/ios/). Fortunately, good guys at [pristine.io](https://pristine.io) has a [CocoaPod](https://cocoapods.org/pods/libjingle_peerconnection) which exactly does that and adds a static library to the project, so TLKSimpleWebRTC using it as everyone else.

TLKSimpleWebRTC was also depending on AZSocketIO as defined in the `.podspecs`, but I changed it with "Socket.IO-Client-Swift" and as you can understand from the pod name, it's written in Swift.

This is where things got complicated; CocoaPods requires `use_frameworks!` for Swift, but this flag can't handle linking static libraries. I couldn't find any workarounds for doing this automatically, therefore I ended up with suggesting very ugly and not version-control-friendly way to add it to the project.

**To sum up**; using SimpleWebRTC is pretty easy on the server / browser side. With iOS things get a bit uglier, but at least I was able to create something working. Updated Socket.IO version of iOS library can be seen [here](https://github.com/ujell/TLKSimpleWebRTC).
