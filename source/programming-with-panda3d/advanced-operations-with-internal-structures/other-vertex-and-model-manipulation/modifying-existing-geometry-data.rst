.. _modifying-existing-geometry-data:

Modifying existing geometry data
================================

If you want to load a model and operate on its vertices, you can walk through
the vertices as shown in
:ref:`the previous section <reading-existing-geometry-data>`, but you should
substitute modifyGeom(), modifyVertexData(), and modifyPrimitive() for
getGeom(), getVertexData(), and getPrimitive(), respectively. These calls
ensure that, in case the data happens to be shared between multiple different
GeomNodes, you will get your own unique copy to modify, without inadvertently
affecting other nodes.

If you want to modify the vertex data, you have two choices. The simplest
option is to create a new :ref:`geomvertexdata` and fill it up with your new
vertex data (as described in :ref:`creating-and-filling-a-geomvertexdata`),
and then assigning this data to the geom with the call geom.setVertexData().
You must ensure that you add enough vertices to the new GeomVertexData to
satisfy the GeomPrimitives that reference it.

Your second choice is to modify the vertex data in-place, by operating on the
existing vertices. You can do this with a
:ref:`GeomVertexWriter <more-about-geomvertexreader-geomvertexwriter-and-geomvertexrewriter>`.
For instance, if you want to copy the (X, Y) position of each vertex to its
(U, V) texture coordinate, you could do something like this:



.. only:: python

    
    
    .. code-block:: python
    
        texcoord = GeomVertexWriter(vdata, 'texcoord')
        vertex = GeomVertexReader(vdata, 'vertex')
        
        while not vertex.isAtEnd():
          v = vertex.getData3f()
          texcoord.setData2f(v[0], v[1])
    
    




.. only:: cpp

    
    
    .. code-block:: cpp
    
        GeomVertexWriter texcoord = GeomVertexWriter(vdata,"texcoord");
        GeomVertexReader vertex = GeomVertexReader(vdata,"vertex");
        
        while(!vertex.is_at_end()){
          LVector3f v = vertex.get_data3f();
          texcoord.set_data2f(v[0], v[1]);
        }
    
    


Important! When you are simultaneously reading from and writing to the same
GeomVertexData object, you should create all of the GeomVertexWriters you need
before you create any GeomVertexReader. This is because of Panda's internal
referencing-counting mechanism; creating a GeomVertexWriter may automatically
(and transparently) force a copy of the data in the GeomVertexData, which
could invalidate any GeomVertexReaders you have already created.

Writing to a column with a GeomVertexWriter does require that the
GeomVertexData's format already has the appropriate columns to handle the data
you are writing (in the above example, for instance, the format must already
have a 'texcoord' column, or the above code will fail). Furthermore, the
columns must have the appropriate format. For instance, if you wanted to
upgrade a model's texture coordinates from 2-D texture coordinates to 3-D
texture coordinates, simply calling texcoord.setData3f(u, v, w) wouldn't
change the fact that the existing texcoord column is a 2-component format; you
would just be trying to stuff a 3-component value into a 2-component column.

If you want to add a new column to a GeomVertexData, or modify the format of
an existing column, you will have to create a new :ref:`geomvertexformat` that
includes the new column (see :ref:`defining-your-own-geomvertexformat`), and
then change the format on the GeomVertexData via vdata.setFormat(format). This
call will internally adjust all of the data to match the new format. (Because
of this internal adjustment, it is important to do this before you create the
first GeomVertexWriter or GeomVertexReader.)
