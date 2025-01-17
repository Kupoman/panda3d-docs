.. _ode:

Using ODE with Panda3D
======================

Panda3D also provides integration for the Open Dynamics Engine. This is a
platform-independent open-source physics engine with advanced types and
built-in collision detection. Panda3D provides support for ODE because
sometimes Panda's limited built-in physics system might not always be enough
to suit more complex needs.

From Panda3D version 1.5.3, ODE support is included in the downloadable
binaries. Before that version, you would have to download the source packages
and include ODE yourself.

This section will explain how to use this ODE system with Panda3D.

Table of Contents
-----------------


a. [[Worlds, Bodies and Masses]]
b. [[Simulating the Physics World]]
c. [[Attaching Bodies using Joints]]
d. [[Collision Detection with ODE]]


More information
----------------


-  The `API
   reference <https://www.panda3d.org/reference/python/namespace!panda3d.ode>`__
   can list the classes and methods available (all of the classes are prefixed
   with Ode), although the function descriptions are lacking. It might also be
   useful to look at the `PyODE API
   reference <http://pyode.sourceforge.net/api-1.2.0/public/ode-module.html>`__,
   which uses very similar class and method names.
-  Developers from Walt Disney VR Studio have held a lecture about using the
   ODE system with Panda3D. Click
   `here <http://video.google.com/videoplay?docid=6429726243770234547&hl=en>`__
   to watch a video recording of it. (Recorded June 18, 2008)


.. toctree::
   :maxdepth: 2

   worlds-bodies-and-masses
   simulating-the-physics-world
   attaching-bodies-using-joints
   collision-detection-with-ode
