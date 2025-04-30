---
title: Making Multiplayer work across different Unity Projects
date: 2024-05-02 09:00:00 +0200
categories: [Unity, Mirror, Photon, Fish-Net, Multiplayer]
tags: [Unity, Multiplayer]   
---

This is a very niche problem, but one that you may face, especially if you are developing a multi-user application that spans different types of devices. 

Unity's multiplayer frameworks, like [Netcode](https://unity.com/products/netcode), [Mirror](https://mirror-networking.com/) or [Fish-Net](https://assetstore.unity.com/packages/tools/network/fish-net-networking-evolved-207815) usually use the asset id defined in the .meta files to synchronise values between clients. This makes a lot of sense, since this identifier is unique and they need a way to keep track of unique objects per client. And in most cases this is fine: you can just change your target platform or have different compiler arguments for different platforms and use the same prefabs for your objects. But sometimes, especially in XR, this is not possible: software libraries you use for an iPad, VisionPro, Quest, SteamVR, HoloLens and Pico usually do not play nicely together, forcing you to create multiple projects. This means that your .meta files, even if you copy and paste them, may not have matching asset ids, so your framework will not be able to match them. 

I was first confronted with this in 2017 during my master's thesis, then more recently in the [IndustrieStadtpark](https://www.5gtroisdorf.de/) project. So here are some possible solutions I've played around with:

- My initial reflex during my Master's time was "whatever, I will write my own networking library". I even built a demo version and all, and it was a good experience, but I also realised that there are way too many details to handle, and it is not worth the effort. 
- Then I had the idea of forking one of the network libraries and allowing overriding asset ids. Easier said than done, and after digging deep into the Mirror source code, I realised it was not worth the effort. 
- I wrote a small Python script to synchronise .meta files between 2 project folders and update all references in .scene and .prefab files. This worked well for cold testing, but was cumbersome and unreliable during real development because Unity kept changing asset ids. 
- Use Photon, it already allows you to override the network id. This was good enough for my master thesis, Photon is also a great networking solution, so this was it, until IndustrieStadtpark came along and using Photon was not possible for some unrelated "reasons". 
- Isolate your network code and assets, create a repository with them and add it as a git submodule to your project. This worked fine, until I have updated the Unity version in only one project. The older version one kept changing the asset ids, so I went one step further:
- Isolate your network code and assets, create a Unity package and import it into your projects instead. Unity keeps .meta files intact when importing the package, so your assets can now be shared between different projects. 

Or maybe use OpenXR and have one project to rule them all (unless you are using VisionPro), that might also work :) 