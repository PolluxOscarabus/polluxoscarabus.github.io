---
layout: post
title: Study of Unity's Physics System
---

# Post section
## Concepts

* Rigidbody component enables physical behavior to the GameObject it is attached to.
* A Collider component allows the GameObject to be moved by collisions by defining its shape representing it during physical collision.
    * Mesh Colliders can be used to match the shape of the object's mesh.
* Colliders added to a GameObject with a Rigidbody component are called *dynamic Colliders*.
* Colliders added to a GameObject without a Rigidbody component are called *static Colliders*.
* Static Collider are used for immobile GameObject that can detect collision and are not affected by physics.
* Dynamic non-kinematic Collider are used for GameObject that can detect and respond to collision and that are affected by physics, and can be moved by forced applied from a script.
* Dynamic kinematic Collider are used for GameObject that can detect collision but won't respond to it nor be affected by physics, and are moved only from a script modifying its Transform component.
*

## Notes

* A GameObject with a Rigidbody component shouldn't be moved from a script by changing its Transform properties to avoid the overhead of having the attached Colliders recalculating their positions relative to the Rigidbody.
* Changing the isKinematic value of a Rigidbody component should be used sparsely due to its high performance cost.
* Mesh Collider are unabled to collide with other Mesh Collider unless set as Convex.
* A good general rule is to use Mesh Collider for scene geometry, and compound primitive colliders for moving objects.
* Static Collider should not be reposition due to the high performance cost of the recalculation.
* Unlike static Colliders, dynamic kinematic Colliders apply friction to other GameObjects, and can wake-up other Rigidbodies.
* Trigger events are only sent if one of the GameObjects with the Colliders component also has a Rigidbody component.

## Examples

## References
[Unity Doc : Colliders](https://docs.unity3d.com/Manual/CollidersOverview.html)

---
# Draft section
## Temporary notes

* Physics Material of GameObject should be set to correctly represent the properties of its material during a collision
* Ragdoll effect is obtained by setting a kinematic Rigidbody animated by a script or animation to non-kinematic to allow physics to now determin its movements (ex : after an explosion).

## ToDo
