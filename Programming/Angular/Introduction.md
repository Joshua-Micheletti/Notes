Angular is a framework for building websites. Angular pages are single page websites (routing).
It's structured in **components**, similar to React, but has a deeper integration with its html component.

Angular works off of **Typescript**, unlike other frameworks built on top of Javascript.

## New app
To create a new angular app, you use the command:
`ng new app-name`

Flags can be added to this command to setup the project:
- `--no-strict`
	- makes the angular build not in strict mode
- `--standalone false|true`
	- choose the standalone mode for the project
- `--routing false|true`
	- choose if the application uses internal routing or not

After creating the project, the Angular CLI will ask you for the style of CSS and if you want to enable server side rendering (SSR) and static site generation (SSG)
## Local server
To run a local server of the website for debugging and testing purposes, use the command:
`ng serve`

This will start a localhost server on the port **4200** that updates as the content of the application is saved.

## Startup
The startup procedure of Angular consists in loading the content of the index.html template and executing the main.ts script.

The main.ts script bootstraps the AppModule, which internally bootstraps the AppComponent, which represents the app-root component.
## Components
To define a new component in the application, use the command:
```node
ng generate component <component-name>
```

This will create 3 (4) files in a new folder with the name of the component:
- component-name.component.html (**template**)
	- defines the html elements to be rendered in the component
- component-name.component.css (**style**)
	- defines the style of the component elements
- component-name.component.ts (**logic**)
	- defines the logic of the component

The **app.component** represents the root component, all user created components are added here, rather than directly to the main index.html template.

Keep components in a folder named like the component itself.

To access newly created components, you need to add the component import to the module of the application (app.module.ts) by adding it in the declarations field.

### component-name.component.ts
#### Decorator
To define a typescript class as an Angular component, you need to provide the @Component [[Decorators]] before the class definition
#### export
In order for other code in Angular to reference and use the component, we need to export the component class. Here is where we define the logic inside the component and all the component's behaviour.

```typescript
export class componentName {
	// logic here
}
```
Remember that in Angular, components are just classes
### component-name.component.css
This is where the style of the component is placed. In angular, css is local to the template of the component, there is no override of other classes or rules.

This is called **style incapsulation**.

### component-name.component.html
This is the template of the component.
HTML pages are first parsed through angular to resolve all the dependencies inside them (like all forms of binding, string interpolation etc...).

An additional feature available through HTML templates in angular is the usage of **Local References**:
This consists in giving a local reference to an HTML tag to be used inside the template (ONLY inside the template) to pass information between components.
If you wanted to pass this information to the typescript logic, you would have to do so through a function taking the HTML component as an argument:

```HTML
<app-component #componentReference></app-component>
<app-component-two (click)="onClick(componentReference)"></app-component-two>
```
### Importing a component
To utilize a component from inside another component or the root app, you need to first import it by referencing its location in the project:
```typescript
import { Component } from './component.component';
```

Once the component is imported, it needs to be added in the imports field of the component decorator of the parent component: (**this goes for standalone components**)
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

#### Dynamic Data
To render HTML based on the content of the component, we use {{}}:
```HTML
<app-component> {{expression}} </app-component>
```
This feature is called **Data Binding** (in particular: **String Interpolation**).
The expression inside String Interpolation **needs to return a string**.
The expression can be anything coming from the typescript bound to the component, a field, a variable, a function or a typescript expression.

The opposite is also true, it's also possible to bind an event from the DOM to a specific typescript function (**Event Binding**):
```HTML
<app-component (event)="expression"></app-component>
```
A very important keyword given is the `$event` keyword, which can be passed as a parameter of a function inside the `""`. It's an object that contains information about the event that was just triggered.


It's also possible to bind a property (**Property Binding**) of an HTML component to an expression from the typescript component:
```HTML
<app-component [property]="expression"></app-component>
```
The expression doesn't need to be a String Interpolation, so no need for {{}}.
The whole `[property]="expression"` is not something of HTML, even if the property could be an HTML tag property, the syntax makes angular handle the evaluation of the property internally.

It's also possible to do **Two-Way-Binding**, where the data is directly accessible and modifiable by the HTML component:
```HTML
<app-component [(ngModel)]="data"></app-component>
```
For this type of binding, importing the **FormsModule** is necessary (found in '@angular/forms').

Two-way-binding makes the data be bound on both ends, so one change on one end causes a change on the other end and vice-versa.
## Interfaces
Like in OOP, interfaces in Angular define the structure of a data type or class.

To create an interface, use the command:
```bash
ng generate interface interface-name
```

## Module
The angular project contains a module file called `app.module.ts`, this file is where we define the modules that are part of our angular application.

To make imports be globally available, we add the imports in this file and list the wanted modules in the imports field of the @NgModule decorator:

```Typescript
import { ModuleOne } from 'sourceOne';
import { ModuleTwo } from 'sourceTwo';

@NgModule({
	...
	imports: [
		ModuleOne,
		ModuleTwo
	],
	...
})
export class AppModule { }
```