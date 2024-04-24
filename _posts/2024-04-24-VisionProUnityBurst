---
title: "Quick solutions for \"xcrun: error: SDK "xros" cannot be located\""
date: 2024-04-24 12:30:00 +0200
categories: [Unity, VisionPro]
tags: [Unity, VisionPro]   
---

As of 24.04.24, if you are using Unity 2022.3.19f1 and Xcode 15.3 (non-beta), Vision Pro builds might fail with the following error:

    Library/Bee/artifacts/iOS/AsyncPluginsFromLinker: xcrun: error: SDK "xros" cannot be located

This happens because of [Burst](https://docs.unity3d.com/Packages/com.unity.burst@1.0/manual/index.html), weirdly production version of Xcode build tools do not include the necessary files, despite having them since Xcode 15.1 beta 1. However there are 2 straightforward solutions for this:

- Use the beta version of Xcode. Simply download and install Xcode 15.4 from developer center, install VisionOS simulator, then make sure that under "*Settings->Locations*", command line tools are set to the beta version.
- Disable Burst on Unity. Burst settings are located under "*Project Settings -> Burst AOT settings*". 

Theoretically, you could manually copy and paste the necessary files from the beta command line tools to the production version. However, this approach might break some other stuff, so would not be advisible. 