## Dropdown Directive closing from anywhere
```Typescript
import {Directive, ElementRef, HostBinding, HostListener} from '@angular/core';

@Directive({
	selector: '[appDropdown]'
})
export class DropdownDirective {
	@HostBinding('class.open') isOpen = false;
	@HostListener('document:click', ['$event'])
	toggleOpen(event: Event) {
	this.isOpen = this.elRef.nativeElement.contains(event.target) ? !this.isOpen : false;
	}
	
	constructor(private elRef: ElementRef) {}
}
```
## Get a copy of an array
```Typescript
array.slice();
```

## Instantly assign a property to a component
```Typescript
constructor(private property: any) {}
```
This will declare a private property automatically.

## Turn an array into a list of items
Similar to the unpacking operator of python (`*`) except in typescript it's defined with the `...` operator:
```Typescript
array = [1, 2, 3];
second_array = [5, 7, 6];

second_array.push(...array);

// second_array = [5, 7, 6, 1, 2, 3]
// if we were to use push without the ... operator, the result would be:
// second_array = [5, 7, 6, [1, 2, 3]]
```
## LocalStorage
Browsers provide a local storage (as well as cookies) to store data between sessions of an application.
Anything can be stored there, but there's a size limit (i think, needs double checking).

Remember that the data stored there can only be of type `string`, so if you want to store an object, remember to `stringify` it first:
```Typescript
localStorage.setItem('data', JSON.stringify(object));
```
This storage is visible in the browser dev tools.

`localStorage` is an object globally available in Javascript.
