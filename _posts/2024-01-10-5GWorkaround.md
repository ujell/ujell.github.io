---
title: Have you tried to turning it off and on again?
date: 2024-01-10 09:00:00 +0200
categories: [Unity, Trivial]
tags: [Unity]   
---
In the [5G Troisdorf](https://www.5gtroisdorf.de/) project, we have a remote collaboration case between Quest Link based VR and a HoloLens. Now details of the system are not that much important here (although you can check some parts of it from [this paper](https://dl.acm.org/doi/10.1145/3544549.3585835)), but the gist of it is that both HoloLens and VR apps are connected to each other with [MixedReality-WebRTC](https://github.com/microsoft/MixedReality-WebRTC) based video chat, and to the central server via web sockets and [Mirror](https://mirror-networking.com/).

The problem happens when the VR user unfocuses the app. For some reason MixedReality-WebRTC can not handle this and disconnects, web socket disconnects and Mirror drops the connection, all seemingly independent of each other. I've spent some time trying to identify why these were happening, but quickly realized even if I figured it out, solutions were not gonna be trivial ones. I've got a huge backlog of other, less edge-casey bugs that I'd rather spend my time on, so I came up with another idea.

The good thing about working on a research project is that it does not have to be "App Store ready", it's the Wild West. As long as it does what it is supposed to do in the official evaluation and preferably also during the public demos, some weird, unusual stuff here and there are fine. This also means, if it comes to that, there is nothing wrong with just "turning it off and on again":

    private void OnApplicationFocus(bool focus)
    {
        #if !UNITY_EDITOR && !UNITY_WSA
        if (!focus)
        {
            TurnItOffAndOnAgain ();
        }
        #endif
    }

    private void TurnItOffAndOnAgain()
    {
        System.Diagnostics.Process.Start(Application.dataPath + "/../IndustrieStadtpark.exe");
        Application.Quit();
    }

So now each time a user Alt+Tabs, or switch to another display, the app just restarts itself. Yeah not the most elegant solution, but works like a charm! 