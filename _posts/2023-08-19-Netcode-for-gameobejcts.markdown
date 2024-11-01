---
title: "Netcode for Game Objects and Lobby"
categories:
  - Netcode
  - Mobile
tags:
  - Multiplayer
  - Netcode
---

With my work in game design and a small interest in creating a multiplayer game, I searched for a couple of solutions to help me easily connect two game scenes together and have both players moving at the same time.

Around this time, Unity's Netcode for Game Objects was officially released and it piqued my interest. With this, I decided to develop a small game that allowed two mobile devices to connect with each other and fly around a space field.

## Netcode for Game Objects

Going into this, I was worried that I didn't understand enough about networks and multiplayer in general to be able to use anything effectively. However, I found that using Netcode for Game Objects was super easy and I was able to create a working prototype relatively well.

One of the main things that was really useful was the ability to create a simple host-client connection through the Unity servers. One client calls the create room function and the others are able to join with a generated room code. Simple and efficient.

The game will then automatically create a player object for each connected client and track their transforms, allowing each client to see the other client's player.

<iframe width="560" height="315" src="https://www.youtube.com/embed/18e9TeNkOnY?si=FSfJSNLb5GRCjDoA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## Small Side Code that I Worked On

While developing the multiplayer system, I created a small "lock-on" system that used a boxcast from the screen and Cinemachine's group targets.

The main problem I had with the lock-on system was that the lock-on was too sensitive or not sensitive enough. However, with the boxcast lock-on, I was able to create a system that felt nice.

<iframe width="560" height="315" src="https://www.youtube.com/embed/S7Q4OwK6wjA?si=2erpynZbQ8ah0kKE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>