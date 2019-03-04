---
title : "Rooms Transition System"
date  : 2019-02-28T14:51:06+01:00
menu  : "main"
author: Michele Rossi
draft : true
---

## Region and One Giant Room

```
What is a region? When? Where? Why?
```

Region is a subroom

__Coupling between regions and many objects (camera_obj, switch_obj...)__
__Talk about all_switches_enabled in Giant Room__

Pros

* I can see everything w/o switching between rooms
* Regions are versatile in the room's role and they are highly customizable

Cons

* Room editor is laggy as fuck: level design becomes a frustrating experience (slow editing/creation of new levels)
* Change room order is a boring and bugprone process
* No need another abstraction over room (region)
* Rooms are native mechanism in GM

### One level per room

```
What is a transition? When? Where? Why?
```

transition_obj is a teleport from a room to another.

__Talk about all_switches_enabled in One Level Per Room__


```
#region create-event

// Must be set in Instance Creation Code
target_room = noone;
spawn_x     = noone;
spawn_y     = noone;

```
When actor collides with it, switch room to target_room at positition [spawn_x, spawn_y]:

```
#region step-event
if (place_meeting(x, y, actor_obj))
{
	if (room_exists(target_room))
		room_goto(target_room);
		
	with (actor_obj)
	{
		spawn_x = other.spawn_x;
		spawn_y = other.spawn_y;
	}
}
```

In the player_obj's room start event:

```
x = spawn_x;
y = spawn_y;
```

Pros

* No more lag
* More control on engine's workflow (e.g. switch room order is way faster then drag and drop game objects)
* Cleaner code

Cons

* Split geometry in several rooms can be a problem
* Transitions between rooms must be placed contiguous

### FX

