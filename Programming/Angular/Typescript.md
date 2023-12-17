This note is here to write certain properties of Typescript that are worth remembering.
## Constructor shortcut
You can write fields of a class inside the constructor parameters, and as long as they are prefixed with an identifier (`private`, `public`), they will automatically get initialized and become accessible as normal fields of the class:
```Typescript
class Class {
	constructor(
		public field1: number,
		private _field2: string
	) {}

	function() {
		this.field1 = 5;
		this._field2 = "hello";
	
		console.log(this.field1);
		console.log(this.field2);
	}
}
```
## Getter
You can set a **getter** method inside a class, this method will be accessible by the outside as a property (without function call), but inside it will instead execute the content of the getter method for that property and return the return value of the getter method. This is obviously used to **read** fields.
```Typescript
class Class {
	constructor(
		public field1: number,
		public field2: string,
		private _field3: number
	) {}

	get field3() {
		return(this._field3);
	}
}

object: Class = new Class();

console.log(object.field3);
```
## Setter
You can set a **setter** method inside a class, this method will be accessible by the outside as a property (without function call), but inside it will instead execute the content of the setter method for that property. This is obviously used to **write** to fields.
```Typescript
class Class {
	constructor(
		public field1: number,
		public field2: string,
		private _field3: number
	) {}

	set fieldOne(value: number) {
		this.field1 = number;
	}
}

object: Class = new Class();

object.fieldOne = 10;
```
## Converting string to number
It's possible to quickly convert a string holding a number into a number by adding a `+` sign before the string:
```Typescript
let value = '2000';
console.log(value);  // "2000"
console.log(+value); //  2000
```