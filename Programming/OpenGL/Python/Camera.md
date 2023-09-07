Virtual cameras are a composition of about 5 vertices, a few parameters and a View Matrix.

# Vertices
## Position
Position vector represents the position of the camera in 3D space, to move around, we need to modify this vector.

## Front
Vector that represents the direction of the camera, if we want to move forward, we need to move following the front vector:

```python
class Camera:
...
	# function to move the camera forwards and backwards
	def move(self, amount):
		self.position += amount * self.front
		self._calculate_vectors()
...
```

This is also the vector we update when we want to look towards another direction:

```python
import glm
import math

class Camera:
...
	# function to turn around the camera
	def turn(self, yaw, pitch):
		# update the turn angles of the camera
		self.yaw += yaw
		self.pitch += pitch

		# clamp the pitch so the camera can't tilt too much
		self.pitch > 89.0:
			self.pitch = 89.0
		self.pitch < -89.0:
			self.pitch = -89.0

		# calculate the direction vector based on the new turn angles of the camera
		direction = glm.vec3(
			math.cos(glm.radians(self.yaw)) * math.cos(glm.radians(self.pitch)),
			math.sin(glm.radians(self.pitch)),
			math.sin(glm.radians(self.yaw)) * math.cos(glm.radians(self.pitch))
		)

		# update the front vector
		self.front = glm.normalize(direction)
		self._calculate_vectors()
...
```

The turn angles are usually calculated based on the movement of the mouse cursor on the screen compared to the last frame.

Yaw and Pitch are the angles that represent the angles the camera is turned and pitched at.

## World Up
Usually this vector is set to (0.0, 1.0, 0.0), since the Y axis is usually the vertical axis.

This vector represents the direction of the up vector in the world reference.

You can lift the camera by this vector similarly to the previous movement example by moving by this vector.

## Right
Vector that represents the vector coming out of the right of the camera.
This is useful to be able to move the camera left and right (in a similar manner to the forward/backwards and the up/down movement).

This vector can be calculated based off the previous vectors:
``` python
import glm

class Camera:
...
	def _calculate_vectors(self):
		self.right = glm.normalize(glm.cross(self.world_up, self.front))
		...
```

## Up
Vector that represents the vector coming out of the top of the camera, not necessarily lined up with the World Up vector.

``` python
import glm

class Camera:
...
	def _calculate_vectors(self):
		...
		self.up = glm.cross(self.front, self.right)
		...
```

# View Matrix
This matrix contains all the information about the camera, and is used to multiply the model vertices to obtain the movement of the camera.

``` python
import glm

class Camera:
...
	def _calculate_vectors(self):
		...
		self.view_matrix = glm.lookAt(self.position,
						              self.position + self.front,
						              self.world_up)
```

The arguments of the `glm.lookAt` function represent the position of the camera, the direction and the reference to the up vector in the world.

# Implementation
This is an example of an implementation of a basic lookaround camera for 3D

```python
import glm
import math

# class to implement a 3D virtual camera
class Camera:
	# constructor method
	def __init__(self):
		# postion vector
		self.position = glm.vec3(0.0, 0.0, 3.0)
		# direction vector
		self.front = glm.vec3(0.0, 0.0, -1.0)
		# world reference vector
		self.world_up = glm.vec3(0.0, 1.0, 0.0)

		# right vector, calculated based on the others
		self.right = glm.normalize(glm.cross(self.world_up, self.front))
		# up vector, calulated based on others
		self.up = glm.cross(self.front, self.right)

		# initialize the turn angles
		self.yaw = -90.0
		self.pitch = 0.0

		# calculate the view matrix
		self.view_matrix = glm.lookAt(self.position,
									  self.position + self.front,
									  self.world_up)

	# method to recalculate the vertices and view matrix
	def _calculate_vectors(self):
		self.right = glm.normalize(glm.cross(self.world_up, self.front))
		self.up = glm.cross(self.front, self.right)
		self.view_matrix = glm.lookAt(self.position,
									  self.position + self.front,
									  self.world_up)

	# method to move the camera forwards and backwards
	def move(self, amount):
		self.position += amount * self.front
		self._calculate_vectors()

	# method to strafe the camera left and right
	
	def strafe(self, amount):
		self.position += amount * self.right
		self._calculate_vectors()

	# method to lift the camera up and down	
	def lift(self, amount):
		self.position += amount * self.world_up
		self._calculate_vectors()

	# method to turn the camera depending on the angle
	def turn(self, yaw, pitch):
		# add up the new orientation
		self.yaw += yaw
		self.pitch += pitch

		# clamp the pitch
		if self.pitch > 89.0:
			self.pitch = 89.0
		if self.pitch < -89.0:
			self.pitch = -89.0

		# calculate the direction vector
		direction = glm.vec3(
			math.cos(glm.radians(self.yaw)) * math.cos(glm.radians(self.pitch)),
			math.sin(glm.radians(self.pitch)),
			math.sin(glm.radians(self.yaw)) * math.cos(glm.radians(self.pitch))
		)
	
		# update the front vector
		self.front = glm.normalize(direction)
		# recalculate the rest of the vectors and view matrix
		self._calculate_vectors()

	# obtain a formatted version of the view matrix ready for opengl uniforms
	def get_ogl_matrix(self):
		return(glm.value_ptr(self.view_matrix))
```