---
title: "Quest 3: Navmesh Baking with Room Scene"
categories:
  - MixedReality
  - VR
tags:
  - XR
  - MR
---

The room scene is the core of XR development in the Quest 3 and creates planes for each of the walls in the scene. While creating my village game, I decided it would be nice to use a Nav mesh so the enemies can track down villagers. 

# Baking Nav Meshes During Runtime
Nav Meshes are usually baked/created before runtime and inside the editor. However, since I do not have access to the room layout until runtime, there is no way for me to bake a mesh before hand. Luckly, I found a stable “experemental” package from Unity that allows Nav Meshes to be baked during runtime. 

# Weird things with Nav Mesh
I attached the Nav Mesh component to the floor object and ran the BuildNavMesh() using a script. This should bake the mesh at that moment and create a Nav mesh for me to use. However, for some reason, the Nav Mesh wasn't anywhere to be seen. Searching the room, I found the mesh baked into the wall instead of the floor. I found this very strange but then figured I should try running the function on the walls and see if it would bake it into the floor. Sure enough, this worked.

![Quest3-Navmesh]({{ "../assets/images/Quest3-Navmesh.PNG" | relative_url }})

