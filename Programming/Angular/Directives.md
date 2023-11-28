Directives are **classes** that add additional behavior to elements HTML from angular
You can only use 1 structural directive per HTML component (except for variables).
## ngFor
Used for **repeating** data dynamically in a template (basically for loop for html)
This can render multiple elements of the DOM that change by a small parameter.
```HTML
<app-component
	*ngFor="let element of list"
	[parameter]="element">
</app-component>
```
This code will create a component for each element of the list


## Variables
It's possible to declare variables inside the HTML tag of a component's template in Angular.
This is done with the keyword: **let-variable** 