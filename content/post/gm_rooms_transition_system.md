---
title : "Rooms Transition System"
date  : 2019-02-28T14:51:06+01:00
menu  : "main"
author: Michele Rossi
draft : true
---

Transition is a game object that is used for navigations between levels.

---

Every transition must:

* switch to a specific room
* set a spawn position for the player.
* performs an animation (e.g. slide)

When the player and the transition collides, we set the transitions in active state and the switch happens.

```
#region create-event

enum TransitionSlide
{
	None,
	Top,
	Bottom,
	Left,
	Right
}

// Must be set in Instance Creation Code
target_room = noone;
spawn_x     = noone;
spawn_y     = noone;
slide       = TransitionSlide.None;

persistent        = true; // required
cur_frame         = 0;
max_frames        = 30;
prev_room_surface = noone
active            = false;
```

Why we need transition instances to be persistent? From GameMaker documentation:
"A persistent instance is one that will be 'carried over' from room to room, meaning (for example) that it only has to be created once at the start of the game and it will be present in all further rooms."
Transition instances are persistent because they performs the animation when the room that instantiated them is released.

```
#region step_event

with (actor_obj)
{
	if (place_meeting(x, y, other.id))
	{
		if (room_exists(other.target_room))
		{
			// Change room
			room_goto(other.target_room);
			
			// Set new spawn position
			spawn_x = other.spawn_x;
			spawn_y = other.spawn_y;

			// Fx
			prev_room_surface = surface_create(window_get_width(), window_get_height());
			surface_copy(prev_room_surface, 0, 0, application_surface);
			active = true;
		}	
	}
}

if (active)
{
	cur_frame++;
	if (cur_frame > max_frames)
	    instance_destroy();
}
```

At animation's end, transition's instance is destroyed.

```
#region drawgui-event

if (cur_frame <= 0)
	return;

var _offset = round(window_get_width() / max_frames) * cur_frame;
if (surface_exists(prev_room_surface))
{
	switch (slide)
	{
		case TransitionSlide.Top:
			draw_surface(prev_room_surface, 0, _offset);
			break;
		case TransitionSlide.Bottom:
			draw_surface(prev_room_surface, 0, -_offset);
			break;
		case TransitionSlide.Left:
			draw_surface(prev_room_surface, _offset, 0);
			break;
		case TransitionSlide.Right:
			draw_surface(prev_room_surface, -_offset, 0);
			break;
		case TransitionSlide.None:
			break;
	}
}    
```

Why transition is drawned on gui space?


```
#region cleanup-step

surface_free(prev_room_surface);
```

Data structures must be released manually