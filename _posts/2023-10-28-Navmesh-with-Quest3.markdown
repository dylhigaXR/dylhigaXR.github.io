---
title: "Quest 3: Navmesh Baking with Room Scene"
categories:
  - MixedReality
  - VR
tags:
  - XR
  - MR
---

The room scene is the core of XR development in the Quest 3 and creates planes for each of the walls in the scene. While creating my village game, I decided it would be nice to use a Navmesh so the enemies can track down villagers.

# Baking Navmeshes During Runtime
Navmeshes are usually baked/created before runtime and inside the editor. However, since I do not have access to the room layout until runtime, there is no way for me to bake a mesh beforehand. Luckily, I found a stable “experimental” package from Unity that allows Navmeshes to be baked during runtime.

# Weird Things with Navmesh
I attached the Navmesh component to the floor object and ran the `BuildNavMesh()` using a script. This should bake the mesh at that moment and create a Navmesh for me to use. However, for some reason, the Navmesh wasn't anywhere to be seen. Searching the room, I found the mesh baked into the wall instead of the floor. I found this very strange but then figured I should try running the function on the walls and see if it would bake it into the floor. Sure enough, this worked.

![Quest3-Navmesh]({{ "../assets/images/Quest3-Navmesh.PNG" | relative_url }})

# Update 10/30
After further testing the Navmeshes, I've come to realize that despite the gizmos not showing anything when baking a Navmesh into the floor, you still have to bake a Navmesh into the floor for it to work. However, just baking the Navmesh into the floor still doesn't work and you still need to bake one into the walls.