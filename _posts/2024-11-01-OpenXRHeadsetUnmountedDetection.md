---
title: Detecting headset unmount with OpenXR
date: 2024-11-01 09:00:00 +0200
categories: [Unity, OpenXR]
tags: [Unity, OpenXR]   
---
This might be obvious to some, but for someone unfamiliar with the new input system, like me, it can be a bit confusing.

# Background
If you do public events and show XR demos, you know the drill: show the demo, get the device back, reset the app, and give it to the next person. In our current  [WIRKsam](https://wirksam.nrw/) project, most of our demos are planned to be public, so I wanted to build a way to handle pass-overs gracefully. The main idea is to reset the app automatically when the user takes the headset off. If you're using Oculus libraries, it’s as simple as this:

    OVRManager.HMDMounted += () => { /* Do stuff */ };
    OVRManager.HMDUnmounted += () => { /* Do stuff */ };

However, in this project, I am using OpenXR, which requires a bit more effort.

# Binding the Action
As of the OpenXR 1.11 package in Unity, `OpenXRUtility.IsUserPresent` returns whether the user is wearing the headset or not, as long as it is supported by the device, which is a good start. However, unlike Oculus, OpenXR does not define a similar event, so you’ll need to listen for it as an input.

First, open your `inputactions` file and add an action. Set the type to "Button." Then select  <No Binding>, extend Path, and select XR HMD -> Usages and User Presence. Make sure to select "XR" in the control schemes.

![Action](https://ujell.github.io/assets/img/20241101/Action.png){: width="600"}

![Binding](https://ujell.github.io/assets/img/202411101/Binding.png){: width="600}

# Listening the Action
Here’s a component for listening to the action:

    using UnityEngine;
    using UnityEngine.InputSystem;

    public class PresenceListener : MonoBehaviour
    {
        public InputActionAsset inputActions;

        private InputAction userPresenceAction;

        private void OnEnable()
        {
            userPresenceAction = inputActions.FindAction("UserPresence");
            if (userPresenceAction != null)
            {
                userPresenceAction.performed += OnDeviceMounted;
                userPresenceAction.canceled += OnDeviceUnmounted;
                userPresenceAction.Enable();
            }
        }

        private void OnDisable()
        {
            // Unsubscribe when the object is disabled or destroyed
            if (userPresenceAction != null)
            {
                userPresenceAction.performed -= OnDeviceMounted;
                userPresenceAction.canceled -= OnDeviceUnmounted;
                userPresenceAction.Disable();
            }
        }

        private void OnDeviceMounted(InputAction.CallbackContext context)
        {
            // Do sth
        }

        private void OnDeviceUnmounted(InputAction.CallbackContext context)
        {
            // Do sth
        }
    }

# Small warning
This, of course, still depends on if the device supports the feature or not. In my experience, it works for Quest Pro and Quest 3, but other devices might give different results. Also, make sure to use the latest OpenXR package, as previous versions had some reported bugs related to this feature.