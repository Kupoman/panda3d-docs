.. _the-scene-graph:

The Scene Graph
===============

The Scene Graph: a Tree of Nodes
--------------------------------

Many simple 3D engines maintain a list of 3D models to render every frame. In
these simple engines, one must allocate a 3D model (or load it from disk), and
then insert it into the list of models to render. The model is not "visible"
to the renderer until it is inserted into the list.

Panda3D is slightly more sophisticated. Instead of maintaining a list of
objects to render, it maintains a tree of objects to render. An object is not
visible to the renderer until it is inserted into the tree.

The tree consists of objects of class
``PandaNode``. This is actually a
superclass for a number of other classes:
``ModelNode``,
``GeomNode``,
``LightNode``, and so forth.
Throughout this manual, it is common for us to refer to objects of these
classes as simply nodes. The root of the tree is a node called
``render``. (Note: there may be
additional roots for specialized purposes, like
``render2d`` for 2D objects)

Panda3D's "tree of things to render" is named the scene graph.

What you Need to Know about the Hierarchical Scene Graph
--------------------------------------------------------

Here are the most important things you need to know about the hierarchical
arrangement of the scene graph:

#. You control where objects go in the tree. When you insert an object into
   the tree, you specify where to insert it. You can move branches of the tree
   around. You can make the tree as deep or as shallow as you like.
#. Positions of objects are specified relative to their parent in the tree.
   For example, if you have a 3D model of a hat, you might want to specify
   that it always stays five units above a 3D model of a certain person's
   head. Insert the hat as a child of the head, and set the position of the
   hat to (0,0,5).
#. When models are arranged in a tree, any *rendering attributes* you assign
   to a node will propagate to its children. For example, if you specify that
   a given node should be rendered with depth fog, then its children will also
   be rendered with depth fog, unless you explicitly override at the child
   level.
#. Panda3D generates bounding boxes for each node in the tree. A good
   organizational hierarchy can speed frustum and occlusion culling. If the
   bounding box of an entire branch is outside the frustum, there is no need
   to examine the children.


Beginners usually choose to make their tree completely flat--everything is
inserted immediately beneath the root. This is actually a very good initial
design. Eventually, you will find a reason to want to add a little more depth
to the hierarchy. But it is wise not to get complicated until you have a
clear, specific reason to do so.

NodePaths
---------

There is a helper class called
``NodePath`` which is a very small
object containing a pointer to a node, plus some administrative information.
For now, you can ignore the administrative information; it will be explained
in a :ref:`later section <instancing>` of the manual. It is the intent of the
panda designers that you should think of a NodePath as a handle to a node. Any
function that creates a node returns a
``NodePath`` that refers to the
newly-created node.

A NodePath isn't exactly a pointer to a node; it's a "handle" to a node.
Conceptually, this is almost a distinction without a difference. However,
there are certain API functions that expect you to pass in a NodePath, and
there are other API functions that expect you to pass in a node pointer.
Because of this, although there is little conceptual difference between them,
you still need to know that both exist.

You can convert a NodePath into a "regular" pointer at any time by calling
``nodePath.node()``. However, there is no
unambiguous way to convert back. That's important: sometimes you need a
NodePath, sometimes you need a node pointer. Because of this, it is
recommended that you store NodePaths, not node pointers. When you pass
parameters, you should probably pass NodePaths, not node pointers. The callee
can always convert the NodePath to a node pointer if it needs to.

NodePath methods and Node methods
---------------------------------

There are many methods that you can invoke on NodePaths, which are appropriate
for nodes of any type. Specialized node types, like LODNodes and Cameras (for
instance), provide additional methods that are available only for nodes of
that type, which you must invoke on the node itself. Here are some assorted
examples:



.. only:: python

    
    
    .. code-block:: python
    
        # NODEPATH METHODS:
        myNodePath.setPos(x, y, z)
        myNodePath.setColor(banana)
        
        # LODNODE METHODS:
        myNodePath.node().addSwitch(1000, 100)
        myNodePath.node().setCenter(Point3(0, 5, 0))
        
        # CAMERA NODE METHODS:
        myNodePath.node().setLens(PerspectiveLens())
        myNodePath.node().getCameraMask()
    
    




.. only:: cpp

    
    
    .. code-block:: cpp
    
        // NODEPATH METHODS:
        myNodePath.set_pos(x, y, z);
        myNodePath.set_color(banana);
        
        // LODNODE METHODS:
        myNodePath.node()->add_switch(1000, 100);
        myNodePath.node()->set_center(LPoint3f(0, 5, 0));
        
        // CAMERA NODE METHODS:
        myNodePath.node()->set_lens(new PerspectiveLens());
        myNodePath.node()->get_camera_mask();
    
    


Always remember: when you invoke a method of
``NodePath``, you are actually
performing an operation on the node to which it points.

In the example above, we call node-methods by first converting the NodePath
into a node, and then immediately calling the node-method. This is the
recommended style.


.. toctree::
   :maxdepth: 2

   scene-graph-manipulations
   common-state-changes
   manipulating-a-piece-of-a-model
   searching-the-scene-graph
   instancing
