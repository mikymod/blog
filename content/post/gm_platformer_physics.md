---
title : "Gm Platformer Physics"
date  : 2019-03-01T09:49:47+01:00
menu  : "main"
author: Michele Rossi
draft : true
---


------

### Solids And Actors

* Each object are AABB (axed-aligned bounding boxes)
* Object size and position are integers
* Solids are a static geometry 
* Actors are physical object that interacts with level geometry
* Actors and solids never overlap
* Solids never interact with other solids

## Actor Move

* Check collision with solid each pixel

## Solid move

* Solid can carry or push an Actor
* Push always override Carry
