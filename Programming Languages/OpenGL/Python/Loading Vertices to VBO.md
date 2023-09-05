In order to load vertices to VBO in python it's necessary to convert them to the correct format using numpy:

``` python
import numpy as np
from OpenGL.GL import *

# list of vertices
vertices = [-0.5, -0.5, 0.0, 1.0, 0.0, 0.0,
             0.5, -0.5, 0.0, 0.0, 1.0, 0.0,
            -0.5,  0.5, 0.0, 0.0, 0.0, 1.0,
             0.5,  0.5, 0.0, 1.0, 1.0, 1.0]

# conversion of the vertices using numpy
vertices = np.array(vertices, dtype=np.float32)

# generate the VBO (Vertex Buffer Object)
VBO = glGenBuffers(1)
# bind the VBO to be the current one
glBindBuffer(GL_ARRAY_BUFFER, VBO)
# pass the data from the converted vertices to the VBO
glBufferData(GL_ARRAY_BUFFER, vertices.nbytes, vertices, GL_STATIC_DRAW)
```