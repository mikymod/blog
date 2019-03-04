---
title : "Rooms Transition System"
date  : 2019-02-28T14:51:06+01:00
menu  : "main"
author: Michele Rossi
draft : true
---

## Region and One Giant Room

What is a region? When? Where? Why? 

Pros

* I can see everything w/o switching between rooms
* Regions are versatile in the room's role and they are highly customizable

Cons

* Room editor is laggy as fuck: level design becomes a frustrating experience (slow editing/creation of new levels)
* Change room order is a boring and bugprone process
* No need another abstraction over room (region)
* Rooms are native mechanism in GM

### One level per room

What is a transition? When? Where? Why?

Pros

* No more lag
* More control on engine's workflow (e.g. switch room order is way faster then drag and drop game objects)
* Cleaner code

Cons

* Split geometry in several rooms can be a problem
* Transitions between rooms must be placed contiguous


### Implementation

In the create event:

```
enum TransitionDir
{
    Top,
    Bottom,
    Left,
    Right
}

target_room = noone;

```

In the step event:

```
if (place_meeting(x, y, actor_obj))
{
    if (room_exists(target_room))
    {
        room_goto(target_room);
    }
}
```

### Change object position

```
/// @function transition_set_position
/// @arg {object} obj
/// @arg {enum} dir

if (!instance_exists(obj))
    return;

switch (dir)
{
    case TransitionDir.Top:
        obj.y = room_height - 16;
        break;
    case TransitionDir.Bottom:
        obj.y = 16
        break;
    case TransitionDir.Left:
        obj.x = room_width - 16;
        break;
    case TransitionDir.Right:
        obj.x = 16;
        break;
}

```
------