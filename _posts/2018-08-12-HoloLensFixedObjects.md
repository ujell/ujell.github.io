---
title: A small tip about display locked objects on HoloLens
date: 2018-08-12 09:00:00 +0200
categories: [Unity, HoloLens]
tags: [HoloLens, Unity, Canvas, HoloToolkit-Unity, MixedRealityToolkit, Display locked, static]   

---

First of all, as you already probably know, having fixed objects on the HoloLens screen is highly frowned upon and strongly disliked by the users. As a designer or developer, you should try not to have display locked elements, at all.

That being said, in some rare cases it makes sense to have an object or text on the display, independent from the real world. For instance you might want to show a frame at screen borders to help users to adjust the HoloLens -which is especially useful if multiple people who might have none to little HoloLens experience will try the app one after another during a demo. To achieve that, you might write a small script and update the object position inside the `Update()` method, or move the object directly under the camera, or use a canvas. 

However, unless you stay completely still, static objects shake a lot, which is not only nauseating a bit but also breaks the AR illusion.  But I have an easy work around: I use [Solver Radial View](https://github.com/Microsoft/MixedRealityToolkit-Unity/blob/master/Assets/HoloToolkit/Utilities/Scripts/Solvers/SolverRadialView.cs) from MixedRealityToolkit to make these objects a little bit less static but more user friendly.  
If the object is supposed to be in the middle of the screen, like a screen sized canvas, just add `Solver Radial View` script to the object, set maximum view degrees to "1" and set min and max distance to the same value. If the object is supposed to stay at any other position, create an empty game object at the center, make your static object child of this empty game object, arrange the position and than set the z position of the static object to 0. Then add `Solver Radial View` script to the object, set maximum view degrees to "1" and set min and max distance to the desired distance. Complicated? Just check the screenshots: 

<p align="center">
  <img src="https://i.imgur.com/1ynN50E.png" alt = "">
</p>

And here it is during the action:


<p align="center">
  <img src="https://i.imgur.com/K443v9b.gif" alt = "">
</p>
