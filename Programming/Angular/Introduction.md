Angular is a framework for building websites.
It's structured in **components**, similar to React, but has a deeper integration with its html component.

Angular works off of **Typescript**, unlike other frameworks built on top of Javascript.

## Components
To define a new component in the application, use the command:
```node
ng generate component <component-name>
```

This will create 3 (4) files in a new folder with the name of the component:
- component-name.component.html
	- defines the html elements to be rendered in the component
- component-name.component.css
	- defines the style of the component elements
- component-name.component.ts
	- defines the logic of the component

### Component-name.component.ts

#### @Component
This is where the component gets created through the use of the `@Component` decorator:
this decorator specifies the metadata for the component object that will later be defined
##### selector
This field specifies the name of the component inside the Angular application, it's composed of the prefix "app-" followed by the name of the component itself.
##### standalone
Field that specifies if the component is standalone or not (require NgModule)
##### imports
Field that specifies the requirements for the component, basically the imports used by the component itself 
##### templateUrl
Reference to the HTML file that defines the structure of the component and its HTML elements.
##### styleUrls
Reference to the stylesheet(s) to get style code from, to stylize the HTML elements of the component
#### export
In order for other code in Angular to reference and use the component, we need to export the component class. Here is where we define the logic inside the component and all the component's behaviour.

```typescript
export class componentName {
	// logic here
}
```

### Importing a component
To utilize a component from inside another component or the root app, you need to first import it by referencing its location in the project:
```typescript
import { Component } from './component.component';
```

Once the component is imported, it needs to be added in the imports field of the component decorator of the parent component:
```typescript
@Component({
	...
	imports[
		Component
	],
	...
})
```

Once the component is properly imported, it can be used by adding its selector (the name of the component inside the angular app, defined by the component's own Component decorator) as an HTML tag in the template of the parent component:
```HTML
<app-component></app-component>
```

To render HTML based on the content of the component, we use {{}}:

```HTML
<app-component> {{componentField}} </app-component>
```

## Interfaces
Like in OOP, interfaces in Angular define the structure of a data type or class.

To create an interface, use the command:
```bash
ng generate interface interface-name
```