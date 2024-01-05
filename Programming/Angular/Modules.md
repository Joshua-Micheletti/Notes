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
