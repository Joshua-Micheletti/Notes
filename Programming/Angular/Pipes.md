Pipes are constructs that transform data from one form to another, without changing the original data.
This is especially useful when you want to adapt how data is displayed without changing the underling data structure, or if you need to adapt the data to be compatible with different systems.

In the **Template**, it's possible to use pipes inside string interpolation for example, to format the displayed output:
```HTML
<p>
	{{ data | uppercase }}
</p>
```
To apply a pipe we use the pipe operator `|`, which allows us to **chain** the output of the left most component to the input of the next component, transforming the output with every pipe.
**Pipe order is from Left to Right.**

Angular has some build in pipes, like:
- uppercase
	- transforms the input string into all uppercase
- date
	- transforms the input `Date` into a formatted date
	- takes a string to choose the format of the date
- async
	- transforms the input Promise or Observable into the data that will be received when the Promise or Observable resolves.

**Pipes can be applied to anything, not necessarily just strings in string interpolation.**
## Parameters
Pipes can take parameters to be configured.
These parameters are passed to the pipe separating it with a `:`
```HTML
<p>
	{{ data | pipe:parameter1:parameter2 }}
</p>
```
Here in this example, `parameter1` and `parameter2` are parameters passed to the pipe `pipe` which will affect how the pipe transforms the data.
## Custom Pipes
To define a custom pipe, we just create a new Typescript file called `name.pipe.ts` (following the usual convention) where we use the `@Pipe` [[Decorators]] to define the new Pipe.
This class needs to implement the `PipeTransform` interface and define the `transform` method, which is where the pipe logic goes:
```Typescript
@Pipe({
	name: 'custom'
})
export class CustomPipe implements PipeTransform {
	transform(value: any) {
		// transform the value at will
		return(value)
	}
}
```
The `name` parameter in the `@Pipe` decorator represents the name of the pipe to use in the template.
The `transform` method takes the input value as an argument and needs to return something.

**Remember to add the newly created pipe inside the `appModule` to be able to use it in the project.**

The pipe can also be created through the Angular command:
`ng generate pipe name`

This will automatically create and setup the pipe source and import it in the module.

**Remember that pipes aren't executed every time the data changes, only when the parameters change. This is to prevent re-calculation.**
If you want the pipe to re-calculate every time the input changes, you need to set an additional parameter in the `@Pipe` decorator:
```Typescript
@Pipe({
	name: 'custom',
	pure: false
})
```
By setting the `pure` parameter to `false`, you enable re-calculation on input change.
**Could be inefficient, be careful.**
This type of pipe is called **Impure**
### Parameters in custom pipes
To accept a parameter in a pipe, we add the parameter as an argument of the `transform` method, and use it accordingly inside the pipe logic.

**Remember that the arguments in the `transform` method need to match the order in which they are passed when you use the pipe in the Template**