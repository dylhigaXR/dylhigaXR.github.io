---
title: "Fireball Simulator"
categories:
  - Hand Tracking
  - VR
tags:
  - VR
  - raycast
  - Hand tracking
---

When the Magic Leap 1 first came out, the first thing I tried to do was build an application with their hand tracking and gesture recognition features. This ended up becoming a fireball simulation where the fireball would be shot like you were a wizard. I recently got an Oculus Quest 2 and wanted to see if I could recreate that project in VR.

## Creating the Fireball

The key difference between these two development experiences is the fact that I had access to gesture recognition with the Magic Leap SDK, but nothing with the Oculus Integration from Unity. I figured that using raycasts was the best way to make up for the lack of gesture recognition.

By using the transform of the center of the player's hands provided with `lHandCenter.transform.position` and `rHandCenter.transform.position`, I could shoot raycasts that would tell me when the hands were facing each other.

```csharp
RaycastHit frontHitl;
RaycastHit frontHitr;

if (Physics.Raycast(lHandCenter.transform.position, 
                    lHandCenter.transform.TransformDirection(new Vector3(-0.15f, 1.0f, 0.0f)), 
                    out frontHitl, Mathf.Infinity, layerMask)) {
    if (Physics.Raycast(rHandCenter.transform.position, 
                        rHandCenter.transform.TransformDirection(new Vector3(0.15f, -1.0f, 0.0f)), 
                        out frontHitr, Mathf.Infinity, layerMask)) {
        fireBallClone = Instantiate(FireBall);
        handsFacing = true;
    }
}
```

This leads to:
![RayCastDemo]({{ "../assets/images/RayCastDemo.PNG" | relative_url }})

Then, by calculating the midpoint between 2 points in space with `mid = A.position + (B.position - A.position)/2`:

```csharp
Vector3 middlePos = lHandCenter.transform.position + 
                    (rHandCenter.transform.position - lHandCenter.transform.position) / 2; 
```

The fireball instantiates and stays between the palms.

![FireBallDemo]({{ "../assets/images/FireBallDemo.PNG" | relative_url }})

By using layer masks, the raycasts will only collide with the other hand's collider.

## Shooting the Fireball

Now comes the problem of detecting when the player pushes their hands forward, indicating the fireball will be shot. The best way I could think about doing this was to invert the direction of the raycast and have them search for a hitbox behind the player.

However, I wanted to make it so the player only had a limited time after moving their hands to shoot the fireball.

```csharp
RaycastHit backHitl;
RaycastHit backHitr;

timer += Time.deltaTime;
if (timer - startTime < 0.1) {
    if (Physics.Raycast(lHandCenter.transform.position, 
                        lHandCenter.transform.TransformDirection(new Vector3(0.15f, -1.0f, 0.0f)), 
                        out backHitl, Mathf.Infinity, layerMaskHead)) {
        if (Physics.Raycast(rHandCenter.transform.position, 
                            rHandCenter.transform.TransformDirection(new Vector3(-0.15f, 1.0f, 0.0f)), 
                            out backHitr, Mathf.Infinity, layerMaskHead)) {
            fire = true;
        }
    }
}
```

This is a makeshift timer system that roughly tracks if 1/4 of a second has passed. The raycasts will be shot out from the back of the hands during this time, and if nothing is found then the fireball will be destroyed. However, if the raycasts do hit the player, the fireball will be shot "forward".

In order to simplify the travel direction, I made it so the fireball would always be facing the player by using `LookAt()`. Then, when shot, the fireball would move in its relative backward direction to simulate it being shot forward... oddly enough. If I didn't do this, the ball wouldn't rotate while the player moves and the direction of the ball would...

Thanks for reading!