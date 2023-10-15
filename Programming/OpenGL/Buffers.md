# VBO
VBOs stand for Vertex Buffer Objects, they are memory areas in VRAM, they are used to store vertex data (vertex, normals, uvs, colors)

To load data into VBOs we need to create a new buffer with:

```python
vbo = glGenBuffers(1)
```

We keep track of the ID of the newly generated buffer by passing an empty variable in the arguments or by getting the return value of the function

Once we have created the buffer, we need to bind it:

```python
glBindBuffer(GL_ARRAY_BUFFER, vbo)
```

By calling this function, we set the newly created VBO to the Array Buffer slot, so all the next operations will happen in the bound VBO

Now we can load data into the VBO:
VBOs are similar to C arrays: when they get created, the size of the buffers is set and can't be changed, the only way to store more data in the buffer, is to re-assign the memory of the buffer entirely.

To initialize the VBO, we use the function [glBufferData](https://registry.khronos.org/OpenGL-Refpages/gl4/html/glBufferData.xhtml):

```python
glBufferData(GL_ARRAY_BUFFER,
			 float_size * number_of_elements,
			 data,
			 GL_STATIC_DRAW)
```

The first argument specifies the type of buffer to fill (VBO)
The second argument specifies the amount of memory to occupy in VRAM
The third argument specifies the data to be loaded in the newly occupied memory
The fourth argument specifies how the buffer is gonna be used

The recently filled VBO has a set size, and if we want to change its size, we need to call the glBufferData function again with a different size (second argument).

The previously stored data is forgotten when this is done.

If we want to change the data inside the buffer, it's best to use the function [glBufferSubData](https://registry.khronos.org/OpenGL-Refpages/gl4/html/glBufferSubData.xhtml):

```python
glBufferSubData(GL_ARRAY_BUFFER,
				offset,
				float_size * number_of_elements,
				data)
```

The difference here is that we're just changing the data inside the already filled buffer from the `offset` position to the `offset + float_size * number_of_elements` position with the `data` contents.

VBOs by themselves though aren't enough to access data from the GPU (shaders), for that we need a way to interpret the buffered data.

# VAO
Vertex Array Object. They are objects with **n** amounts of locations, these locations is where VBOs are bound and accessed from shaders.

To create a VAO, we use the function [glGenVertexArrays](https://registry.khronos.org/OpenGL-Refpages/gl4/html/glGenVertexArrays.xhtml):

```python
vao = glGenVertexArrays(1)
```

To setup the VAO, it needs to be bound to the current selected VAO with the function: [glBindVertexArray](https://registry.khronos.org/OpenGL-Refpages/gl4/html/glBindVertexArray.xhtml):

```python
glBindVertexArray(vao)
```

Before using VAO locations, we need to first enable them, with the function [glEnableVertexAttribArray](https://registry.khronos.org/OpenGL-Refpages/gl4/html/glEnableVertexAttribArray.xhtml):

```python
glEnableVertexAttribArray(0)
```

Now that a location has been enabled, we can bind a VBO to it and specify how to interpret the data, with the function [glVertexAttribPointer](https://registry.khronos.org/OpenGL-Refpages/gl4/html/glVertexAttribPointer.xhtml):

```python
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, float_size * 3, void*)
```

The first argument specifies the location to be filled
The second argument specifies the amount of data in one unit of data (1 = float, 2 = vec2, 3 = vec3, 4 = vec4)
The third argument specifies the type of the data
The fourth argument specifies if the data is normalized (range of -1, 1)
The fifth argument specifies how many spaces each data occupies
The sixth argument specifies the offset of the data in the VBO

Now that the VBO is bound to a location in a VAO, it's possible to access the VBO data inside shaders.

Changing the VBO data doesn't require re-executing the VAO setup.