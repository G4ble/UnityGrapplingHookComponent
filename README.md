# Unity GrapplingHook Component
Simple force based grappling hook componet for Unity3d.



## How to use

Add the component to your assets folder, then add the component to the player's GameObject. Under the component in inspector, add the Camera object to the camera_camera option. The component is ready to go.

## How it works

The component checks whether the player has clicked on a grappling node. Holding down left mouse keeps the tether connected and it is disconnected when the player releases left mouse. 



The player can only tether to an object with the tag "grappling_node" and the tether point is at the grappling node's transform.position point. Add some objects with tag "grappling_node" and start swinging!



This component has three modes:

* Ratcheting sets the tether length shorter when the player moves closer to the node. (This mode is the most fluid and fun mode in my opinion)

* Loose only applies the tension force when the player's distance to the grappling node is at the tether length. The tether length is set when the node is first hooked. Think a ball swinging on a rope.

* Rigid keeps the player at a set distance from the grappling node. The tether length is set when the node is first hooked. Think a ball swinging on a metal rod.



The draw_dev_line draws a line from the player's transform.position to the hooked grappling nodes transform.position is selected.

The break_tether_velo option sets a velocity where if a player exceeds, the tether breaks. Set this option to "Infinity" if you do not want this behaviour.

The disconnect_on_los_break option breaks the tether of the los to the grabbling node is broken.

## How it actually works (the physics)

Depending on what mode is selected, a tension force is applied at certain conditions. In modes ratcheting and loose, tension is applied during an frame where the player distance to the grappling node will exceed the tether_distance next update. In rigid mode, tension force is applied every frame.


The tension force is calculated as followed.
* Calculate velocity in units/frame (V_upf)
* Test position = V_upf + current_posision aka (test_pos)
* New position after frame = (text_pos - hooked_node_pos).normalized * tether_distance + hooked_node_pos (new_pos)
* New velocity = new_pos - current_posision (new_velo)

Force = mass * (d_velocity / d_time)

* d_velocity = new_velo - V_upf
* Tension force = mass * (d_velocity / d_time)
* Tension force is applied as impulse (force is only applied this frame)

![how it works](https://i.imgur.com/EZFc2aT.png)

