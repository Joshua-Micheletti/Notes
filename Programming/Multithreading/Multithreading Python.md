In order to achieve multithreading, it's possible to either use the Threading library or the multiprocessing library.

The multiprocessing creates multiple processes while Threading creates threads inside the main process (used for async calculations).

# Multiprocessing
Multiprocessing in Python consists in creating new processes and let the operating system decide its scheduling.

To create a multiprocess program in python, we first need to import the library:

```python
import multiprocessing
```

Once the library has been imported, we can create a new process by creating a new Process object:

```python
process = multiprocessing.Process(target=function, args=(arg1, arg2,))
```

This object is created by passing a reference to the execution function and its parameters.

To wait for the process to finish, we can use the method `join` from the process object:

```python
process.join()
```

To know how many CPU cores are available for multiprocessing, the multiprocessing library gives a method `cpu_count()`.


To share data between processes, it's possible to use a Manager and pass the data structure to the process function as an argument:

```python
manager = multiprocessing.Manager()
return_dict = manager.dict()

process = multiprocessing.Process(target=function,
								  args=(arg1, arg2, return_dict))
```

