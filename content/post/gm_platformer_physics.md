---
title : "Gm Platformer Physics"
date  : 2019-03-01T09:49:47+01:00
menu  : "main"
author: Michele Rossi
draft : true
---


------

## General

* Begin Step: check collisions if needed
* Step: calculate velocities (must be defined in each child object)
* End Step: move object (subpixel movement)

### Solids And Actors

* Each object are AABB (axed-aligned bounding boxes)
* Object size and position are integers
* Solids are a static geometry
* Actors are physical object that interacts with solids
* Solids never interacts with other solids
* Actors and solids never overlap

## Actor Move

* Check collision with solid each pixel

## Solid Move

* Solid move through other solids w/o collides
* Solid can carry or push an Actor
* Push always override Carry

