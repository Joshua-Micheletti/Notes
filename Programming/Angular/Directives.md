Directives are **classes** that add additional behavior to elements HTML from angular.
You can only use 1 structural directive per HTML component (except for variables).
They work as instructions inside the DOM.

The directive with the `*` before mean that they modify the structure of the DOM.
## Structural Directives
They modify the whole structure of the DOM and are preceded by a `*` sign.
The `*` sign creates a version of the code with an `<ng-template>` (which is a directive) with the parameter binding on `[ngFor]` or `[ngIf]`.
```HTML
<app-component *ngIf="condition"></app-component>

<!-- Is the same as: -->

<ng-template [ngIf]="condition">
	<app-component></app-component>
</ng-template>
```
The `*` just allows a more compact version of it, but under the hood it's executed as the bottom version.
### ngFor
Used for **repeating** data dynamically in a template (basically for loop for html)
This can render multiple elements of the DOM that change by a small parameter.
```HTML
<app-component
	*ngFor="let element of list"
	[parameter]="element">
</app-component>
```
This code will create a component for each element of the list.
The syntax of the argument is similar to a for-each:
- we define a temporary variable with `let`, this variable will be the singular element inside a list
- `of` is the iterator on the list of components
- `list` is a list of components

If you want access to the index of the current element, you can add the expression:
```HTML
<app-component
	*ngFor="let element of list; let i = index">
</app-component> 
```
Now the local variable `i` will contain the index of the current iteration.
### ngIf
Used for using conditional logic inside a template. Only render the component if the condition is met.
```HTML
<app-component *ngIf="condition"></app-component>
```

An else component can be added to this directive:
```HTML
<app-component *ngIf="condition; else something"></app-component>
```
Often the "something" can be another HTML element referenced by reference (`#`)
### ngSwitch
This directive acts as a switch feature. First you add the `ngSwitch` property bind to the parent of all the cases specifying the value to switch on.
Then on the children, you use `*ngSwitchCase` depending on the value. And finally you add an `*ngSwitchDefault` to the default value in case the value is never any of the cases:
```HTML
<div [ngSwitch]="value">
	<div *ngSwitchCase="1"></div>
	<div *ngSwitchCase="2"></div>
	<div *ngSwitchDefault></div>
</div>
```
This way only the elements whose value matches the switch value will be created.

### Custom Directive
It's possible to define a user created directive with the `@Directive` [[Directives]]:
```Typescript
import { Directive, Input, TemplateRef, ViewContainerRef } from '@angular/core';

@Directive({
	selector: '[directiveName]'
})
export class DirectiveName {
	@Input() set appDirective(condition: boolean) {
		// directive logic
	}

	constructor(private templateRef: TemplateRef<any>, private vcRef: ViewContainerRef) {}
}
```
To access and modify the DOM from the directive we use the `TemplateRef` and `ViewContainerRef` references, which allows us to add, remove or modify content of the DOM.

To decide on the logic behind the directive, we use an `@Input` parameter which is a `set` function in this case (it's still a parameter, the set keyword just calls the defined function every time the parameter changes).

The condition corresponds to the evaluated expression passed as argument when applying the directive to a component.

## Attribute Directives
They only affect the element they are added to
### ngStyle
Directive to change the style of a component:
```HTML
<app-component [ngStyle]="{backgroundColor: getColor()}"></app-component>
```
Style can be defined inside this directive, the strong feature of this directive is the ability to change the style depending on the component state.

Allows dynamic style change.
This directive needs to be property bound (`[]`) and the argument needs to be a javascript object.
### ngClass
Allows to dynamically change the css class of a component.
```HTML
<app-component [ngClass]="{class: condition}"></app-component>
```
The argument of the directive is a javascript object where the key is the css class name and the value is a condition that returns true or false depending on whether that class should be applied or not
### Custom Directive
It's possible to build your own custom attribute directives by creating a new directive file (conventionally named: `name.directive.ts`) where we define the behavior of the directive:
```Typescript
import { Directive, ElementRef, OnInit, Renderer2 } from '@angular/core';

@Directive({
	selector: '[name]'
})
export class DirectiveName implements OnInit {
	constructor(private elementRef: ElementRef, private renderer: Renderer2) {
	}

	ngOnInit() {
		// do something with the element this directive was attached to
	}
}
```
**Always modify the DOM through the Renderer2 or @HostBinding instead of directly with the ElementRef. Use the ElementRef only to reference which element of the DOM to modify**

To be able to use the directive, it needs to be added to the module decorator's `declarations:` field.

The generation of the directive can also be done automatically with the command:
`ng generate directive directiveName`

Inside custom directives it's possible to listen to elements in the DOM with the @HostListener [[Decorators]] and it's possible to have @Input and @Output parameters to Property bind or Event bind to.

Setting an alias on your @Input / @Output parameters with the same name as the directive, it allows the component to bind to that property/event directly with the name of the directive, similar to how ngStyle and ngClass work.

You can also use property binding in the directive without square brackets `[]` if the argument of the parameter is a string.
### ng-content
This directive is used to pass the body of a component from the parent to the child:
```HTML
<!-- Parent Template -->
<app-child>
	<p>Body</p>
</app-child>
```
```HTML
<!-- Child Template -->
<ng-content></ng-content>
```
Angular will replace the `<ng-content></ng-content>` directive with the body passed by the parent. This way it's possible to place the body coming from the parent wherever you want inside the template of the child.

Without this directive inside the child, the body coming from the parent is immediately discarded.
## Variables
It's possible to declare variables inside the HTML tag of a component's template in Angular.
This is done with the keyword: **let-variable** 