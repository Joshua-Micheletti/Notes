Modules are a way in Angular to split code into logical blocks that provide certain features and components.

All components inside a module are local to the module, and not accessible by other modules (unless imported).

To declare a new module, we use the `@NgModule` [[Decorators]]:
```Typescript
@NgModule({
	declarations: [
		// components available in the module
	],
	imports: [
		// external modules available in the module
	],
	exports: [
		// components to export from this module, when this module is imported
		// in another module, what's written here is what will be available
	]
})
export class Module {}
```
**ngIf and ngFor are provided in the BrowserModule, but that module should only be imported in the AppModule, on every other module, you should import the CommonModule instead.

The only exception to modules and components being local to modules is [[Services]]. Those can be imported once in the `AppModule` and become globally available to all other modules.
## Routing
It's possible to define Module [[Routing]] the same way we do with the `AppModule` for routing.
It works the same way, just that instead of the `.forRoot` method, we use `.forChild`. This will tell Angular that the routes defined in the separate module are now children of the root route.
## Declaration
Components can't be declared in multiple modules inside the same application. This means that if a component is required in more than one module, then there should be a module wrapping that component and declares it, which then is imported by the modules that need that component. Obviously the component needs to be exported by the wrapping module.
## Types
### Feature Module
This module usually wraps components that provide or are part of similar features
### Shared Module
This module usually wraps components that are shared through the application, it's useful for sharing entities and fixes the issue of only being able to declare components only in 1 module.
### Core Module
This module helps making the `AppModule` slimmer: it wraps services in it, so they don't have to all be declared as providers in the `AppModule`. Core modules can wrap services together depending on their functionalities.

`providedIn` already solves this issue, as it allows you to declare services without declaring them in the providers field of the `AppModule` decorator. **`providedIn('root')` is still the preferable way to handle Service injections.**

The core module paradigm is similar to the `RoutingModule` paradigm (outsourcing the configuration into a different module).