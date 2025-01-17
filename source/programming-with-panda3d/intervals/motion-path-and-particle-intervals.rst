.. _motion-path-and-particle-intervals:

Motion Path and Particle Intervals
==================================

MopathInterval
--------------


Motion paths are an advanced feature of Panda3D, and they are discussed later.
Still, motion paths have their own intervals. A motion path interval is much
like a function interval in that there are no additional parameters other than
the motion path and the NodePath it is affecting.



.. code-block:: python

    intervalName = MopathInterval("Motion Path Name", NodePath, "Interval Name")



Read more about :ref:`motion-paths`.

ParticleInterval
----------------


Particle effects can be run from inside intervals as well:



.. code-block:: python

    intervalName = ParticleInterval(
        "Particle Effect Name",
        parent,
        worldRelative = 1,
        loop = 0 or 1,
        duration = myDuration
    )



Read more about :ref:`particle-effects`.
