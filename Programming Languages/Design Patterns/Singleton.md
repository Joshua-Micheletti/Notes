# Definition
Singleton design patterns consists of creating an object whose reference doesn't change throughout the program execution, it's always retrievable and there can only be one instance of the object.

This design pattern is especially useful if you want to have a global reference to an object, without having an actual global variable floating, or having to pass a reference to an object every time.

# Implementations
## [[Python]]
Implementation through the metaclass approach:
``` python
# Singleton.py
class Singleton(type):
    """
    The Singleton class can be implemented in different ways in Python. Some
    possible methods include: base class, decorator, metaclass. We will use the
    metaclass because it is best suited for this purpose.
    """

    _instances = {}

    def __call__(cls, *args, **kwargs):
        """
        Possible changes to the value of the `__init__` argument do not affect
        the returned instance.
        """
        if cls not in cls._instances:
            instance = super().__call__(*args, **kwargs)
            cls._instances[cls] = instance
        return cls._instances[cls]
```

The ``_instances`` private variable holds the instances of the class.
The ``__call__`` method makes the class a callable class and checks if the instance of the object already exists, if it already exists, then return the instance, otherwise create a new instance.

Any class that needs to be a Singleton can inherit this class as a metaclass to gain its properties:
``` python
# Window.py
from singleton.Singleton import Singleton

class Window(metaclass=Singleton):
```

``` python
# Renderer.py
from singleton.Singleton import Singleton

class Renderer(metaclass=Singleton):
```

``` python
# main.py
window1 = Window()
window2 = Window()
renderer = Renderer()

if id(window1) == id(window2):
	print("window objects are the same object")

if id(window1) != id(renderer):
	print("pog, window and renderer aren't the same")
```

As seen in the demo, the window objects created with the same constructor reference the same object, while the renderer object results different, despite inheriting the same metaclass.