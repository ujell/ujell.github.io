---
title: How to save object position on HoloLens
date: 2017-04-02 09:00:00 +0200
categories: [Unity, HoloLens]
tags: [HoloLens, Unity, Anchor, World Anchor, HoloToolkit, Emulator]   

---
Saving positions of holograms is most of the time a must for a usable application. It sounds complicated but actually simpler than one would think. Besides a HoloLens development environment you need to;

1. Read  "Spatial anchors" [section](https://developer.microsoft.com/en-us/windows/mixed-reality/spatial_anchors) from Microsoft's mixed reality guide to understand what's going on behind the scenes.
2. Add [HoloToolkit-Unity](https://github.com/Microsoft/HoloToolkit-Unity) or [WorldAnchorManager](https://github.com/Microsoft/HoloToolkit-Unity/blob/master/Assets/HoloToolkit/Utilities/Scripts/WorldAnchorManager.cs) script and it's dependencies to your project.

When you got everything ready, add `WorldAnchorManager` to your scene. It doesn't actually matter much where to but to keep things tidy, it's better to add it under a new game object called "Anchor Manager".

After `WorldAnchorManager` is added all you have to do is following code from a script that attached to your gameObject;

    WorldAnchorManager.Instance.AttachAnchor(gameObject, anchorName);

And if you don't want to save the position of that gameObject anymore, you should call;

    WorldAnchorManager.Instance.RemoveAnchor(gameObject);

If you try this now, you will realize you can't move your objects anymore. Weird, uh? Well, since you associated certain position with your object, HoloLens assures that it stays there. To avoid this, remove anchor before moving the object, and add it back with the same anchor name afterwards. For example, if you are using HandDraggable script from HoloToolkit-Unity you can do it like this;

    void Start () {
        // Adding anchor initially
        WorldAnchorManager.Instance.AttachAnchor(gameObject, anchorName);

        gameObject.GetComponent<HandDraggable>().StartedDragging += () =>
        {
            // Removing anchor temporary when dragging started
            WorldAnchorManager.Instance.RemoveAnchor(gameObject);
        };
        gameObject.GetComponent<HandDraggable>().StoppedDragging += () =>
        {
            // Adding back with the same name when dragging ended
            WorldAnchorManager.Instance.AttachAnchor(gameObject, anchorName);
        };
    }

Also don't forget to check best practices document at [Windows Dev Center](https://developer.microsoft.com/en-us/windows/mixed-reality/spatial_anchors)
