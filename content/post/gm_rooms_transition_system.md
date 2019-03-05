---
title : "Rooms Transition System"
date  : 2019-02-28T14:51:06+01:00
menu  : "main"
author: Michele Rossi
draft : true
---

## Regions and One Giant Room

* `Why use a single giant room?`
* `What is a region? When? Where? Why?`

Initially, I decided to use a single room for an entire area in my platformer because i want to see
each level in the editor.
Region is a subroom and it was used as a replacement of original room.
Each object, that normally interact with room, has to interact with region instead.
Transition happens when player change region.
Camera is anchored to the current region.
Each piece of code becomes too complex because of this.

For example:

```
/// @function all_switches_enabled
/// @arg region

with (argument0)
{
	var instances = ds_list_create();
	collision_rectangle_list(x1, y1, x2, y2, switch_obj, false, true, instances, false);
	
	var all_enabled = true;
	for (var i = 0; i < ds_list_size(instances); i++)
	{
		var door_switch = instances[|i];
		if (!door_switch.enabled)
			all_enabled = false;
	}
	
	ds_list_destroy(instances);
		
	return all_enabled;
}
```

instead of:

```
/// @function all_switches_enabled

with (switch_obj)
{
	if (!enabled)
		return false;
}

return true;
```

Pros

* I can see everything w/o switching between rooms
* Regions are versatile in the room's role and they are highly customizable

Cons

* Room editor is laggy as fuck: designing levels becomes a frustrating experience
* Change room order is a boring and bugprone process
* Code is a mess
* No need another abstraction over room (region)

### One level per room

```
What is a transition? When? Where? Why?
```

Transition is a teleport from a room to another.

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

```
#region create-event

// Must be set in Instance Creation Code
target_room = noone;
spawn_x     = noone;
spawn_y     = noone;

// Fx
persistent = true; // when changing room keep this object alive
cur_frame = 0
max_frames = 45
prev_room_surface = noone
```

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

    // screenshot
    prev_room_surface = surface_create(room_width, room_height);
    surface_copy(prev_room_surface, 0, 0, application_surface);
}

cur_frame++;
if (cur_frame > max_frames)
    instance_destroy();
```

```
#region drawgui-event

if (cur_frame)
{
    // Right transition
    var _offset = round(room_width / max_frames) * cur_frame;
    if (surface_exists(prev_room_surface))
        draw_surface(prev_room_surface, _offset, 0);
}
```

```
#region cleanup-step

surface_free(prev_room_surface);
```