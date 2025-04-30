---
title: Trying HoloToolkit sharing examples
date: 2016-10-09 09:00:00 +0200
categories: [Unity, HoloLens]
tags: [HoloLens, Unity, Sharing, HoloToolkit, HoloToolkit-Unity]   
---
Getting started with HoloToolkit sharing services is a bit overwhelming, mainly because it's divided into both in HoloToolkit and HoloToolkit-Unity repositories.
# Server  
First of all, Microsoft doesn't provide compiled version of the server, so you'll need to compile it yourself. Fortunately, they provide all the necessary scripts and guides to do this; basically clone the main [HoloToolkit repository](https://github.com/Microsoft/HoloToolkit) and follow the instructions on the [getting started guide](https://github.com/Microsoft/HoloToolkit/blob/master/Sharing/Src/Source/Docs/ExtendedDocs/GettingStarted.md) to setup the build environment then run "BuildAll.bat" script.

If you already updated to Visual Studio 2017 and don't have VS2015 anymore, this part is a bit annoying. VS2017 changed some paths that build scripts [depend](https://stackoverflow.com/questions/42805662/vsvars32-bat-in-visual-studio-2017) and you either have to change scripts by hand, then edit all the solutions and upgrade their targets, or simply install VS2015. I tried the first option, but compiling takes lots of time and at some point installing VS2015 just became more viable solution.

While compiling I also made a huge rookie mistake, didn't add Java paths and spent some time to understand what's wrong. So following "Setup the Build Environment" part of the "Getting Started" guide word-by-word is to your advantage.

When the compiling is done, there will be a new folder called *SDK* on your *Sharing* folder. Open cmd or PowerShell and go to *Server* folder under the *SDK*. You have to options to run the server;
* **SharingService.exe -local** : Runs the server like any other program. Server is closed when you close the command prompt.
* **SharingService.exe -install** : Installs the server as a service and constantly runs it on the background. Note that you might get "Error 5 : Access Denied" for this command, following this StackOverflow [answer](https://stackoverflow.com/a/17056440/1311234) for *Server* folder with using "SERVICE" instead of "SYSTEM" keyword solves the issue.

 So we run the service, but how can we check if it actually works? Open *SDK/Tools/SessionManager* folder, select the appropriate architecture for your computer and run *SessionManager.UI.exe*. This is a small tool that helps you to control currently running sessions and connected devices, it'll be really handy while you are first setting your Unity scene for the sharing.

 <p align="center">
   <img src="https://i.imgur.com/xRw7Jh5.png" alt ="">
 </p>

# Client

Open HoloToolkit-Unity project and open one of the examples, preferably "SharingTest" for the first try. All 3 Tests are explained [here](https://github.com/Microsoft/HoloToolkit-Unity/blob/master/Assets/HoloToolkit/Sharing/README.md#tests). Cool thing is, sharing library works on Unity Editor as well as the emulator, so you don't need to deploy the app to HoloLens for testing.

Play the scene on the Unity Editor, if everything was correct, you'll see something like this;

<p align="center">
  <img src="https://i.imgur.com/7x39f5h.png" alt ="">
</p>

But what if everything wasn't correct? *SharingStage* script, which is located under the *Sharing* prefab has a property called "*Show Detailed Logs*". For the "SharingTest" scene it's on by default, if not, enable it and follow your logs for finding the issue. You can also use *SessionManager* tool to check if the service is running and your app is connected to it.

If this scene works, you can also test the "SharingSpawnTest" scene, which adds / removes prefabs on / from the scene with voice commands on HoloLens and keyboard inputs on Unity Editor ("spawn basic" (**I**), "spawn custom" (**O**) and "delete object" (**M**)). Before creating VS project for the emulator, beware that you should change "*Server Address*" property of *SharingStage* script in *Managers/Sharing* object. HoloLens emulator lives in a virtual machine, hence has it's own localhost and you need to use your computers IP address to ensure communication between the emulator and the server.

Where to go from here? Well, the best tutorial on this so far is at Microsoft's [Mixed Reality Academy](https://developer.microsoft.com/en-us/windows/mixed-reality/holograms_240). Besides that, there aren't many sources on this topic yet, so for now you'll need to explore the HoloToolkit yourself.
