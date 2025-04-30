---
title: Testing HoloLens app in Unity Editor
date: 2017-03-17 09:00:00 +0200
categories: [Unity, HoloLens]
tags: [HoloLens, Unity, Unity Editor, HoloToolkit, Emulator]   
---
If you recently started exploring HoloLens development and followed [Holographic Academy tutorials](https://developer.microsoft.com/en-us/windows/holographic/academy) you might have realized testing small stuff, like changing position of a game object, can get cumbersome very quickly. First you need to export a Visual Studio solution, then compile the solution and deploy your app to the Emulator or the real device. Depending on the project and your computer's configuration, this might take a while and get frustrating pretty quickly.

<p align="center">
  <img src="https://imgs.xkcd.com/comics/compiling.png" alt="">
</p>

I was having the same problem and there are 2 things I discovered that didn't explicitly mentioned neither in tutorials nor in Microsoft's developer [guides](https://developer.microsoft.com/en-us/windows/holographic/unity_development_overview). Firstly, you don't have to export a Visual Studio solution every time you only changed a script, you can just compile and test with your existing solution.

But the second thing is absolute time saver. Microsoft has several open source projects ([[1]](https://github.com/Microsoft/HoloToolkit) [[2]](https://github.com/Microsoft/HoloToolkit-Unity) [[3]](https://github.com/Microsoft/HoloLensCompanionKit)) on HoloLens that includes bunch of useful script and prefabs like head stabilizer or cursors. One of the projects, [HoloToolkit-Unity](https://github.com/Microsoft/HoloToolkit-Unity), comes with Unity specific tools, such as in editor controls. So when you follow the instructions on [Getting Started Guide](https://github.com/Microsoft/HoloToolkit-Unity/blob/master/GettingStarted.md) and added HoloToolkit-Unity to your project, you can test your changes directly on the editor without going through deployment process, voila!

Of course there are some caveats, for example audio input does not work. Moreover I have encountered some rare cases that worked on the editor but not on the device. But regardless, it's the best way right now to have fast iterations on your code and scene.

On a side note; HoloToolkit-Unity also has a build window that allows you directly deploy your app to the device or the emulator -which is also quite helpful.
