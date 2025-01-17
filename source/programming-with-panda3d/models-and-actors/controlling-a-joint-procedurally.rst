.. _controlling-a-joint-procedurally:

Controlling a Joint Procedurally
================================

Sometimes one wishes to procedurally take control of a model's joint. For
example, if you wish to force a character model's eyes to follow the mouse,
you will need to procedurally take control of the neck and head. To achieve
this, use controlJoint.



.. code-block:: python

    myNodePath = actor.controlJoint(None,"modelRoot","Joint Name")



This creates a dummy node. Every frame, the transform is copied from the dummy
node into the joint. By setting the transform of the dummy node, you can
control the joint. Normally, one would want to use
``setHpr`` to rotate the joint
without moving it. The dummy node is initialized in such a way that the joint
is in its default location, the one specified in the model's egg file.

You must store a local (not global) transform in the dummy node. In other
words, the transform is relative to the joint's parent bone. If you are
controlling the forearm of a model, for instance, the transform will be
relative to the upperarm.

The string "modelRoot" represents the name of the model node - the string
"modelRoot" is usually the correct value.

The string "Joint Name" represents the name of the joint. Typically it would
be something like "Femur", or "Neck", or "L Finger1". This is usually set
inside the modeling package. For example, in MAX, each object in the scene has
a name, including the bones. If necessary, you can determine the joint names
by scanning the egg file for strings like
``&lt;Joint&gt; Femur``. You can also use the
call ``actor.listJoints()`` to show the
complete hierarchy of joints.

Cautions and limitations
------------------------


Prior to Panda3D version 1.5, there were several important limitations to
controlJoint(). These have been lifted as of Panda3D version 1.5.

-  In previous versions of Panda, controlJoint only worked when an animation
   is playing on the joint. This is no longer true; controlJoint now takes
   effect whether an animation is playing or not.
-  It used to be important to make all of your controlJoint() calls for a
   particular model before you made the first call to play(), loop(), or
   pose(). This is no longer necessary; you may call controlJoint() at any
   time.
-  In previous versions of Panda, controlJoint could not be undone. Beginning
   in Panda3D version 1.5, you may call releaseJoint("modelRoot", "Joint
   Name") to undo a previous call to controlJoint().

