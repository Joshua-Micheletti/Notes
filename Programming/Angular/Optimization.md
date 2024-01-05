## Lazy Loading
Lazy loading consists in only loading components present in certain routes when we reach them. By default Angular loads every module at startup, which can be pretty expensive.
By dividing the code into [[Modules]], it's possible to enable Lazy Loading, saving time on the first page load and offsetting the loading time to when a module is required.

The opposite of Lazy Loading is **Eager Loading**, which is the default behavior.

For modules to be able to apply Lazy Loading, they need to provide their own routes with the `RoutingModule.forChild()` method.

The routing of the lazy loaded module should now start from `''` as the route location will be defined by the `AppModule` routing:
```Typescript
{
	path: 'feature',
	loadChildren:
		() => import('./feature/feature.module').then(
			(m) => m.FeatureModule
		)
}
```
Usando il metodo `loadChildren` possiamo caricare le componenti presenti nel modulo `FeatureModule` in modo dinamico nel momento in cui raggiungiamo la route corrispondente (`/feature`).

Now that the module is imported dynamically, it should be removed from the imports of the `AppModule`, and any matching non necessary imports between the `AppModule` and `FeatureModule` should be removed, as they would prevent the effect of Lazy Loading.
### Preloading
Lazy Loading speeds up the first loading but ultimately slows down the loading of modules as we reach them, making the user experience potentially worse.

To combat this, we can define the `preloadingStrategy`:
in the routing module where the lazy loaded routes are defined, we can pass a configuration to the `RouterModule` to define the `preloadingStrategy`:
```Typescript
imports: [RouterModule.forRoot(
	appRoutes,
	{preloadingStrategy: PreloadAllModules}
)],
```
The `PreloadAllModules` will try to load all the modules as soon as possible, but not during the first load. It will first load the base `AppModule`, and then once that's loaded and working, it will try to load other modules that are lazily loaded.

The default value is `NoPreloading`, meaning it won't pre-load any module, they will only be loaded once they are reached (if lazy loading is active).