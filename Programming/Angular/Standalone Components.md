Standalone components are components that don't need to be declared inside a module to be used. This will remove some boilerplate code and make the code slimmer.

Luckily this feature is easily convertible from the older version of Angular, so it should be easy to convert.

To declare a component as standalone, add the `standalone` flag in the `@Component` [[Decorators]]:
```Typescript
@Component({
	standalone: true,
	selector: 'app-test',
	templateUrl: './test.component.html',
	styleUrls: ['./test.component.css'],
})
export class TestComponent {}
```

Standalone components can now `import` other standalone components, they can import whole modules and use their exports, and **they don't need to be declared in the `NgModule`, but rather they need to be imported** (if you want them available module wide).

```Typescript
@Component({
	standalone: true,
	imports: [SharedModule, StandaloneComponent],
	selector: 'app-test',
	templateUrl: './test.component.html',
	styleUrls: ['./test.component.css'],
})
export class TestComponent {}
```

**Potentially, [[Directives]] can also be declared as standalone and imported in the specific standalone components that require them.**

If standalone component A imports standalone component B, then B doesn't need to be imported in the `NgModule`, it can be imported into whatever standalone component is required, in this case in A. This idea allows the creation of specific components as just part of other components, rather than module wide components.
## Standalone root
If standalone components allow you to get rid of modules, then there needs to be a way to remove the `AppModule` as well. The way to do it is to change the `main.ts` bootstrap code to bootstrap off of a component rather than from a module:
```Typescript
// Module (non standalone)
// platformBrowserDynamic().bootstrapModule(AppModule)
//   .catch(err => console.error(err));

// component (standalone)
bootstrapApplication(AppComponent);
```

Getting rid of modules is the biggest gain from using standalone components, as they would often build up to be very big files which just acted as a trashcan of components to use module wide.

Of course this gets rid of the module specific features (routing, lazy loading to name a few), but there are techniques to apply the same effects with standalone components.
## Services
In a fully standalone application, [[Services]] can't be `provided` inside modules anymore.
They can still be `provided` inside singular components, but that will create an instance of the service that is scoped to the component and its children.

If you want to `provide` a service in a fully standalone application, you can pass the service to the configuration object of the `bootstrapApplication` function:
```Typescript
bootstrapApplication(AppComponent, {
	providers: [Service]
});
```
Now the service is available application wide.

**Still the best way of making a service available application wide, is by using the configuration object `{providedIn: 'root'}` in the `@Injectable` [[Decorators]].**
## Routing
Since there are no more modules, [[Routing]] needs to happen in a different way.
We can still define routing modules where we export the `RoutingModule` configured with our routes, but now we need to also import the default `RoutingModule` wherever we use any routing specific directive, and we need to specify the routes for the application in the `main.ts` file:
```Typescript
bootstrapApplication(AppComponent, {
	providers: [
		importProvidersFrom(AppRoutingModule)
	],
});
```

The `importProvidersFrom` is an Angular function that allows you to import modules as providers, in this case, we provide the `AppRoutingModule`. This will make any sub route that uses modules work anyways, as well as lazy loading.
## Lazy Loading
Thanks to standalone component, we can do some [[Programming/Angular/Optimization]] with Lazy Loading without having to wrap the component inside a module and lazy loading the whole module. That option still works, in case you want to batch load certain components, but now you can define your lazy loaded component in the routing module as:
```Typescript
const routes: Route[] = [
  {
    path: 'eagerlyLoaded',
    component: EagerlyLoadedComponent
  },
  {
    path: 'lazyLoaded',
    loadComponent: () => import('./about/lazyloaded.component').then(
	  (mod) => mod.LazyLoadedComponent
	)
  },
  {
    path: 'lazyLoadedModule',
    loadChildren: () =>
      import('./module/lazyloaded-routing.module').then(
        (mod) => mod.LazyLoadedRoutingModule
      )
  },
```

There's an alternative way to lazy load a chunk of modules, and it's done by defining a typescript file that defines a `Route[]` containing all the routes.
This can then be imported with the `import` function and the array can be imported.
This allows you to not have to create a module for routing that imports, configures and exports the `RouterModule`:
```Typescript
// ./group/routes.ts
export const GROUP_ROUTES: Route[] = [
    {
        path: '',
        component: HomeComponent
    },
    {
        path: 'other',
        component: OtherComponent
    }
];

// app-routing.module.ts
const routes: Route[] = [
  {
    path: 'eagerlyLoaded',
    component: EagerlyLoadedComponent
  },
  {
    path: 'lazyLoaded',
    loadComponent: () => import('./about/lazyloaded.component').then(
	  (mod) => mod.LazyLoadedComponent
	)
  },
  {
    path: 'lazyLoadedModule',
    loadChildren: () =>
      import('./module/lazyloaded-routing.module').then(
        (mod) => mod.LazyLoadedRoutingModule
      )
  },
  {
    path: 'lazyLoadedGroup',
    loadChildren: () =>
      import('./group/routes').then(
        (mod) => mod.GROUP_ROUTES
      )
  },
```
We basically just load the array of routes for lazy loading.