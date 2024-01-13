Signals are an alternative to Angular's built in change detection algorithm.
The internal Angular's change detection is based on Zone.js, and while it's really accurate and automatic, it can also be pretty heavy, both in terms of execution and in terms of memory.

Signals allow the user to have **more control** over the flow of change detection, being able to define when a change should be detected. This allows the developer to build **smaller bundles** with **better performance**, at the cost of having to implement the change logic themselves.
## Creating a signal
To create a signal object to manipulate, we use the `signal` function:
```Typescript
numVar = signal<number>(0);
strVar = signal<string>("Hello World");
```
The argument of the `signal` function is the initial value of the signal.

The type of the data to be contained in the signal can be specified between `<>`
### Setting the value
We can later change the value of the signal in 3 ways:
- `set`
	- this method overrides the internal value of the signal
- `update`
	- this method takes a function that has the previous value of the signal as an argument, and returns the new value of the signal
- `mutate`
	- similar to update but used for values that can be mutated (arrays, objects etc...)
	- this is not available in the latest versions of angular

Main difference between `update` and `mutate` is that update needs to return a new value, potentially based on the previous one, it can't modify the existing value directly. Mutate on the other hand, can perform some changes on the existing value without creating a new one.
### Getting the value
To get the value of a signal object, you need to call the signal as a function, and it will return the value of the signal:
```Typescript
this.numVar();
this.strVar();
```
This is also how you get the value of the signal inside the template, for example in string interpolation.
## Specific signals
Angular implements some signals that already provide built in functionalities that works with other signals (similar to pipes for [[Observable]] in rxjs).
### computed
This signal takes a function that is executed whenever the signals inside the function change, and stores the return value of the function inside the signal, to be read elsewhere:
```Typescript
testSignal = signal<number>(0);
doubleSignal = computed(() => this.testSignal() * 2);
```
The function defined inside `doubleSignal` will be executed each time `testSignal` will change, and will update the value of `doubleSignal` with the computed value of the argument function.
This is very similar to the `map` operator.
### effect
This signal takes a function that is executed whenever the signals inside the function change. The return value isn't stored inside the signal itself (i think):
```Typescript
testSignal = signal<number>(0);
logSignal = effect(() => console.log(this.testSignal()));
```
Every time the `testSignal` is changed, the function inside the `effect` signal will be called and executed.
This is very similar to the `tap` operator.