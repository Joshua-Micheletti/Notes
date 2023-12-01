Directives are **classes** that add additional behavior to elements HTML from angular.
You can only use 1 structural directive per HTML component (except for variables).
They work as instructions inside the DOM.

The directive with the `*` before mean that they modify the structure of the DOM.
## ngFor
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

## ngIf
Used for using conditional logic inside a template. Only render the component if the condition is met.
```HTML
<app-component *ngIf="condition"></app-component>
```

An else component can be added to this directive:
```HTML
<app-component *ngIf="condition; else something"></app-component>
```
Often the "something" can be another HTML element referenced by reference (`#`)

## ngStyle
Directive to change the style of a component:
```HTML
<app-component [ngStyle]="{backgroundColor: getColor()}"></app-component>
```
Style can be defined inside this directive, the strong feature of this directive is the ability to change the style depending on the component state.

Allows dynamic style change.
This directive needs to be property bound (`[]`) and the argument needs to be a javascript object.
## ngClass
Allows to dynamically change the css class of a component.
```HTML
<app-component [ngClass]="{class: condition}"></app-component>
```
The argument of the directive is a javascript object where the key is the css class name and the value is a condition that returns true or false depending on whether that class should be applied or not
## Variables
It's possible to declare variables inside the HTML tag of a component's template in Angular.
This is done with the keyword: **let-variable** 