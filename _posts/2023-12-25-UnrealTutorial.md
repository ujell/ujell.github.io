---
title: Fixes and workarounds for Unreal Stack-O-Bot tutorial
date: 2023-12-25 09:00:00 +0200
categories: [Unreal, Tutorial]
tags: [Unreal, Tutorial]   
---
While I was following the Stack-O-Bot [tutorial](https://dev.epicgames.com/community/learning/tutorials/e2V/your-first-game-in-unreal-engine-5), I came across some small issues and inconsistencies. I've realized there are other people asking about these, so here are the ones I've encountered.

> I am using Unreal Engine 5.3.2.
{: .prompt-info }

### Importing texture crashes Unreal
Right in the beginning, while importing `T_Grunge_A` and `T_Plastic_N` textures, my editor kept crashing. Unreal has a Plugin called Oodle to optimize textures in the project, which was causing this issue. Navigating to Edit -> Plugins, searching Oodle and disabling 2 found plugins solved the issue for me.

Weirldy enough I couldn't reproduce this while writing the blog but then also later happened while importing the `T_RockTileable_BC`, so at least for the tutorial I'd suggest to keep those plugins disabled.

### SM_Crate does not have vertex values
When creating the materials for the crate, tutorial makes use of vertex colors to make it multi-colored. However, even if you followed everything correctly, you will notice the center part of your crate, unlike the tutorial, has only one color.

![Crate](assets/img/20231225/crate-vertex.png){: width="400" height="400" }

This is not an issue with your project, but the asset itself: the crate model in the initial ZIP file simply does not have the required vertex values. To solve this, you can make use of the existing Stack-O-Bot project. First delete the asset from your main project. Then if you don't have it alreaedy, download Stack-O-Bot from market place and open it. Find the crate in the content drawer, right click it, select Asset actions -> Migrate. Follow the interface to it to your project, but be careful to select only the model and none of the materials. Finally in your project, re-assign the materials to the crate.

Note that this new create already has the nanite support, which will be officially added later during the tutorial. And tutorial also showcases how migration works during "Vegatation" section, after 00:26:00 time mark. 

### Landscape is not being rendered
Alright this one is actually not a common problem, but something an Unreal beginner like me can do by mistake easily. On the landscape painting part of the project, after creating named rerouts, my landscape looked like this:

![Landscape](assets/img/20231225/landscape.png){: width="300" height="300" }

After dubugging a while, I eventually figured out the error was in output log all along, I was using 4 parameter node instead of 3 while lerping the grass color. Since the material graph did not gave any warning on this, it took me a while to figure it out.

### Character is not rotating to cursor direction
While working on the character movement, you will enable "Orient Rotation to Movement" parameter in Character Movement. However, to make this work, you need to first select BP_Bot, then search for "Yaw" in the Details tab and disable the "Use Controller Rotation Yaw" parameter. 

![Yaw](assets/img/20231225/rotation-yaw.png){: width="300" height="300" }

### Animations only play once
In my case, imported idle, walking and running animations were only playing once. Solution for this is simple, "Loop" parameter in animations were checked out, so enabling it solved the issue. 

### AI Bot does not move automatically
When I have created the BP_AIController blueprint with the given functions, assigned it to new bot and then added to nav-mesh bounds volume, AI bot did not move. After a while I have realized 0,0,0 point - which is the default target of "AI Move To" was out of my nav-mesh since I placed my bot and start point to somewhere random. The bot started moving after using a better coordinate.

### Plate parts have same colors
At the beginning of the "Interaction Component" section, we create a pressure plate using the existing props. Unlike the video, my pressure plate frame and platform props had the same color as the following:

![Plates](assets/img/20231225/plate-colors.png){: width="300" height="300" }

To be honest I could not figure this one out, both of these props are using the same material without any variables. Trying the trick from crate issue also did not work this time -though that was not unexpected. It is not critical and workaround might be using another material, but if you know the solution for this, feel free to reach out! 