---
title: "Quest 3: Train Tracks in the Living Room"
categories:
  - MixedReality
  - VR
tags:
  - XR
  - MR
---
With the improvement of the Quest 3's passthrough quality, the concept of transforming your living space into a playground left room for more, fun ideas. Having played with model trains when I was younger, I thought it would be fun to explore the possibility of laying down tracks in your room without having to worry about cleanup. Thus, I started on a project where you can do just that!

![trainFloor]({{ "../assets/images/trainFloor.PNG" | relative_url }})

# Finding tracks to place
I knew about Unity splines and had learned about Bézier curves during my time studying game engines. However, finding a way to place tracks effectively was going to be challenging. After watching countless YouTube videos and reading the Unity Forums, I stumbled upon an asset I must've acquired through a Humble Bundle Software pack.

[Train Controller (Railroad System)](https://assetstore.unity.com/packages/templates/systems/train-controller-railroad-system-v3-4-116455)

The asset was perfect. It allowed the user to build trains tracks using a spline and extruded a track mesh along that spline. However, there was one problem: it's an editor tool.

# Adjusting Assets
This was discouraging, but it wasn't the end. I spent hours reading through all the scripts and eventually grasped the tool well enough to adjust portions and make it available during runtime. I was able to preview a curve using a line renderer and place a train track using the same curve values. Now that I had the core functions working, it was time to polish the experience to feel like a model train in your room. This meant placing trees and adding track support so the tracks weren't just floating in the air.

![trainFloor2]({{ "../assets/images/trainFloor2.PNG" | relative_url }})

# Hitting the limits
The Quest 3 offers a plethora of development tools, including the ability to test the Scene Room and Scene Mesh through Quest Link. This accelerates the development process but introduced an unforeseen issue. Since Quest Link runs VR off of your PC, the specs differ slightly. While the experience ran smoothly within Quest Link, everything took a nosedive once I built the app and pushed it to the headset.

The experience would pause for half a second when placing tracks and everything, in general, became much slower. As I combed through my project and toggled countless features on and off, I finally concluded that the problem was in the number of raycasts and coroutines I was creating and calling at once. Because everything is called on the same frame the track is placed, the experience is instantiating multiple objects, creating and extruding a new mesh, and calling multiple raycasts to determine the placement of trees.

First, I tackled the raycasts. The only way to fix this was to just reduce the number of them. Although the experience didn't look as nice, being stuck in a frame for half a second was way worse.

Next, every tree that was placed had a coroutine that would give it a little animation. I moved this feature from coroutines to just fit into the update method. This reduced the amount of lag the most and really helped everything go smoothly.

# Developing for stand alones
I really liked working on this project because it made me think a lot about how I went about creating different features. On PC, its easy to completely disregard the impact on performance because its just not noticeable. 