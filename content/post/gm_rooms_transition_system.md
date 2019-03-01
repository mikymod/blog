---
title : "Rooms Transition System"
date  : 2019-02-28T14:51:06+01:00
menu  : "main"
author: Michele Rossi
draft : true
---

### Giant Room

Pros

* I can see everything 

Cons

* room editor is laggy as fuck: level design becomes a frustrating experience (slow editing/creation of new levels)
* rooms are native mechanism in GM
* I can focus on a 

### Transition Object

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