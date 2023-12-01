Decorators are a Typescript feature that provides additional information about the definition of a class or function.
## @Component
This is where the component gets created through the use of the `@Component` decorator:
this decorator specifies the metadata for the component object that will later be defined

This decorator is defined inside **@angular/core**
### selector
This field specifies the name of the component inside the Angular application, it's composed of the prefix "app-" followed by the name of the component itself.
This is how elements are referred to in the template for example:
```HTML
<app-name></app-name>
```
The selector needs to be a unique name, different from normal HTML tags too.

The selector works the same way of a CSS selector. For example, we could add `[]` around the selector string to turn it into a parameter. Now Angular will replace the content of every tag in HTML with that custom attribute with the content of the template.

Same thing for selecting by class, which is done by adding a `.` before the class name.

Only selector that isn't supported by angular is selector by ID, so **no** `#` selector allowed.
### standalone
Field that specifies if the component is standalone or not (require NgModule)
### imports
Field that specifies the requirements for the component, basically the imports used by the component itself 
### templateUrl
Reference to the HTML file that defines the structure of the component and its HTML elements.
The path to the HTML file to use as template needs to be relative to the typescript file location.

If you want to describe the template directly, you can do so by using the field **template** instead and passing a string with HTML code.

This field is 100% **necessary**, every other field is optional but this one is completely necessary.

### styleUrls
Reference to the stylesheet(s) to get style code from, to stylize the HTML elements of the component.

Style can also be done inline like the template.

### encapsulation
This flag sets the `encapsulation` mode for styles for CSS classes and rules.

To change this style, you need to first import `ViewEncapsulation` from `@angular/core` and pass one of its values as an argument to the encapsulation field:
```Typescript
import { Component, ViewEncapsulation } from '@angular/core';

@Component({
	...
	encapsulation: ViewEncapsulation.[None | Emulated | ShadowDom]
})
```
## @Input
This decorator allows a field of a component class to be treated as Input, meaning that its data will be obtained from another component (usually the parent component).

By default properties aren't bindable from outside, they're only usable inside the component.

It's included in the @angular/core module:
```typescript
import { Input } from '@angular/core';
```

To set a field of a component class as Input, just add the decorator before the field definition:
```typescript
@Input() field: Type | undefined;
```

To the right of the field we specify the types accepted by the field's input.

After the field declaration, we can add a `!` to specify that the fields needs to be passed, it can't be left un-initialized, as it won't automatically get a value.

The argument of `@Input()` is an **alias** of the field, this is how the field is referenced by the parents in property binding, but it doesn't change the inside name of the field for the component itself.
### Property binding
To pass a value from a parent component to a child, we pass the data through its html tag:
```HTML
<app-component [inputField]="parentField"></app-component>
```

The content of the parenthesis is a field of the app-component, the content of the string is a value from the parent.

## @Output
This decorator is used to set a field of a component as an output, so that the parent can listen to it and get its data.

This is done through custom events:

```Typescript
import { Component, Output, EventEmitter } from '@angular/core';

@Component()
export class Component {
	@Output() event = new EventEmitter<{field1: number, field2: string}();

	onInternalEvent() {
		this.event.emit({
			field1: 10,
			field2: 'test'
		})
	}
}
```

By setting up an `EventEmitter` as `@Output` we can then pass the event data to the parent listening to that event with **Event Binding**:

```HTML
<app-component (event)="onEvent($event)"></app-component>
```

The behavior of this code is that whenever the function `onInternalEvent` is called, an event is emitted with an object with data. This event is caught by the parent which calls the `onEvent` function with the data object as argument.

Just like in @Input, you can **assign** an alias to the name of the event by passing a string to the argument of the decorator.

## @ViewChild
This decorator allows to access an element of the DOM in the Typescript code:

```Typescript
import { ViewChild, ElementRef } from '@angular/core';
@ViewChild('localReference') childComponent: ElementRef;
```

This `childComponent` object now contains data about the HTML element it referenced.

If you're accessing the component inside `ngOnInit()`, you need to add `@ViewChild('localReference', {static: true})` for it to work as intended.

The content of this reference is made available after the hook `ngAfterViewInit`.
## @ContentChild
This works the same way as `ViewChild` but it's made available after the `ngAfterContentInit` hook instead