Angular is a single page application, it uses routing to simulate multiple URLs while actually staying in the same application.

To add routing functionality to Angular, you need to add the routes on the appModule:
```Typescript
import { Routes, RoutingModule } from '@angular/router';

const appRoutes: Routes = [
	{
		path: '',
		component: HomeComponent
	},
	{
		path: 'something',
		component: SomethingComponent
	}
];

@NgModule({
	...
	imports: [
		RouterModule.forRoot(appRoutes)
	],
	...
})
export class AppModule {}
```
First we create the routes as an array of type Routes composed of multiple objects with a path and a component field.

The path represents the route (`localhost:4200/route`) and the component represents the component that should be loaded when that route is reached. In the object, don't pass the route as `/route` but just as `route`.

After creating the routes, they need to be passed in the modules import of the NgModule by calling the `forRoot` method of the `RouterModule`, and passing the array of routes as an argument.

Now to load the router selected module in the template of the page, we use the `<router-outlet></router-outlet>` directive. This directive will be replaced with the component of the specific route depending on the route you're in.
## Router Link
To navigate the different routes in the application it's possible to use anchor tags with the `href` parameter set to the route `/route`.

This will reload the whole page though, as it literally changes the url to a different page.

To handle routing internally, we can use the [[Directives]] `routerLink`:
```HTML
<a routerLink="/route"></a>
<a [routerLink]="route"></a>
<a [routerLink]="['/route', 'sub']"
```
We can also use Property Binding on it to pass the route as data coming from the component.

To have a better control of the routing, it's also possible to pass an array to the routerLink property directive with following details on the route, like in the third case in the example, which will lead to the route: `/route/sub`

The paths inside the router link are absolute if preceded by a `/` and are relative if lacking the `/`.
For example, a `routerLink="/route"` will always take you to `/route`, while a `routerLink="route"` placed inside a different path, for example `/app` will take you to `/app/route`.

It's also possible to add `../` to navigate upwards in the path.
## routerLinkActive
This directive is triggered when the routing points to the component this directive is attached to and adds a class dynamically depending on the route.
```HTML
<div
	routerLinkActive="class"
	[routerLinkActiveOptions] = "{exact: true}">
	<a routerLink="/route"></a>
</div>
```

This directive has an options directive `routerLinkActiveOptions` which takes an object with an `exact` boolean field, which if set to true, it checks if the whole path matches the one specified in the router link.

This is helpful for root paths which would then be recognized inside of every child path, or for any parent paths.
## Routing Programmatically
It's possible to execute functions that call the router to re-route the user to a different route programmatically. To do this, we need to access the `Router` object and use the `navigate` method:
```Typescript
import { Router, ActivatedRoute } from '@angular/router';
...
constructor(private router: Router, private route: ActivatedRoute) {}

onReRoute() {
	this.router.navigate(['route', '/sub'], {relativeTo: this.route});
}
```

The `navigate` method takes an array of route and sub routes to redirect the page to.

The `navigate` method doesn't know the route it's in, so relative paths don't work normally, for them to work, we need to add a second parameter to configure the navigation.
The second parameter takes an object with fields:
- relativeTo:
	- specifies the relative path for the navigation (`/` by default).
	- the route needs to be specified with a `Route` object, if we want the current route, we use the `ActivatedRoute` object
- queryParams:
	- specifies the query parameters to pass
	- wants an object with key value pairs
	- the parameters are resolved as `?key1=value&key2=value2...`
- fragment:
	- specifies the URL fragment
	- wants a single string (**there can only be 1 fragment**)
	- the fragment is resolved as `#fragment`.
- queryParamsHandling:
	- specifies the behavior upon routing from a route with already existing query params
	- the options are:
		- `drop` (default):
			- the already existing query params will be deleted and replaced with the new ones
		- `merge`:
			- the already existing query params will be added to the new ones
		- `preserve`:
			- keep only the already existing query params
## Routes Parameters
It's possible to define **parameters** to the route during its definition:
```typescript
const appRoutes: Routes = [
	{
		path: '/route/:param',
		component: RouteComponent
	}
];
```

Now that the route has a variable in it, if we access the route `/route/value`, we can then access the **value** bound to the **param** parameter with the `ActivatedRoute` object:
```Typescript
import { ActivatedRoute } from '@angular/router';
...
constructor(private route: ActivatedRoute);

ngOnInit() {
	console.log(this.route.snapshot.params['param']);
}
```
The parameters are stored inside the snapshot.params map, referenced by name.

Multiple parameters can be passed (`/route/:param1/:param2`)

The `snapshot` component is the current state of the URL, this means that if the URL changes without changing the component, the data won't be updated.

If instead we want to react to the parameters changing, we can subscribe to the `ActivatedRoute.params` which is an event that passes the new data every time the parameters change:
```Typescript
import { ActivatedRoute, Params } from '@angular/router';
...
this.route.params.subscribe((params: Params) => { console.log("changed params") })
...
```
This behavior is implemented by **Observables**.

The `Params` object is a map, the parameters are accessed by name (key) as a normal map.

This dynamic approach should be used compared to the other one, especially if you expect your route to change inside a component.

The subscription automatically gets destroyed upon destroying the component.

## Route query and fragment
URLs can also be composed of query `?` and fragment `#` parts.
Those parts can be added to a routerLink by changing the parameters:
```HTML
<a [routerLink]="['/route']" [queryParams]="{query: true}" fragment="fragment">
```
The `queryParams` parameter expects an object with key value pairs and the fragment just expects a string, as there can only be one fragment in the route.

Clicking this link will take you to `/route?query=true#fragment`.

This can also be done programmatically by changing the options of the `router.navigate` method:
```Typescript
this.router.navigate(['/route'], {queryParams: {query: true}, fragment: 'fragment'});
```
Clicking the anchor above or calling this method will have the same effect, they will both take you to the same route with the same parameters.

To retrieve the URL query and fragment parameters it works exactly like for normal parameters:
```Typescript
constructor(private route: ActivatedRoute) {}

// this will access the parameters at the moment of calling
this.route.snapshot.queryParams;
this.route.snapshot.fragment;

// this will call a function everytime the parameters change
this.route.queryParams.subscribe(() => {});
this.route.fragment.subscribe(() => {});
```
Again we just subscribe to the Observable parameters of the `ActivatedRoute` object to detect changes.

**Remember that parsed data from the URL is always a string, so to use it as a number or any other type, it needs to first be converted (for example by adding a `+` before it)**
## Nested routing
In order to organize and to implement routing inside an already existing page / route, you can use nested routing.
You start by defining new routes as children of the parent route:
```Typescript
const appRoutes: Routes [
	{
		path: '',
		component: HomeComponent
	},
	{
		path: 'route',
		component: RouteComponent',
		children: [
			{
				path: 'sub',
				component: SubComponent
			}
		]
	}
]
```

This will create the new route `/route/sub`. Its component will be loaded inside the `<router-outlet>` component in the template of the route component.

This way we can dynamically modify the `<router-outlet>` inside of a component depending on the URL.
## Redirecting
When defining routes it's possible to pass a field called `redirectTo` in order to programmatically change the URL to another one when a certain route is reached.

This behavior is especially useful when defining undefined routes with the **wildcart route**: `**`.
This route will catch any route that isn't defined in the routes object.

An example of redirecting to an error page in case of wrong URL:
```Typescript
const appRoutes: Routes = [
	{
		path: '',
		component: HomeComponent
	},
	{
		path: 'route',
		component: RouteComponent
	},
	{
		path: 'error',
		component: ErrorComponent
	},
	{
		path: '**',
		redirectTo: '/error'
	}
];
```
Now all the invalid routes will be redirected to `/error` to display the ErrorComponent.

Make sure the wildcart route is always the last one in the routing object, otherwise it will catch all routes and make some routes unreachable.

To avoid errors when redirecting to a different path, you should enable the `pathMatch` strategy to `full`, that way the redirection will happen on the full path specified, and it won't be caught by any prefix of the path itself.

```Typescript
{
	path: '',
	redirectTo: '/somewhere-else',
	pathMatch: 'full'
}
```

Without the `pathMatch` option for example, this route would cause an error or always cause redirection, since every other path starts with `''`, and they would all be caught in this path.
## Outsourcing Paths
Usually it's good practice to define the routes in their own module called `app-routing.module.ts`.

In this module we define the routes like normal, we setup the `RoutingModule` with our routes and then we export the configured routing module for other modules in the application to use:
```Typescript
const appRoutes: Routes = [...];

@NgModule({
	imports: [
        RouterModule.forRoot(appRoutes)
    ],
    exports: [RouterModule]
})
export class AppRoutingModule {}
```

Now we can just import this module and use its router module already configured.

## Guards
Guards are pieces of code (usually [[Services]]) that are executed upon entering or leaving a route.
This is useful if there's code that should be executed every time you interact with a route.
### canActivate
This guard executes code every time you enter a route.
To set it up, you need to add the service (or services) that implement the code to run before accessing a route in the `canActivate` field of the routes:
```Typescript
const appRoutes: Routes = [
	{
		route: 'route',
		canActivate: [ActivateGuardService]
		component: RouteComponent
	}
] 
```
The service needs to be setup like this:
```Typescript
export class ActivateGuardService implements CanActivate {
	canActivate(route: ActivatedRouteSnapshot,
				state: RouterStateSnapshot):
				Observable<boolean> | Promise<boolean> | boolean {
		// logic
	}
}
```

The service needs to implement the `CanActivate` interface which defines a `canActivate` method which has:
- Input:
	- ActivatedRouteSnapshot:
		- a copy of the current route information
	- RouterStateSnapshot:
		- a copy of the current router information
- Output (3 possibilities):
	- Observable\<boolean\>:
		- async output to subscribe to
	- Promise\<boolean\>:
		- async output to wait for
	- boolean:
		- sync output

When this method returns true, the route can be accessed, otherwise it stays inaccessible.

If you want to protect just the children of a route, you can use the field `canActivateChild` in the routes definition and implement the `CanActivateChild` interface and `canActivateChild` method.
This will protect all the children of the route this field is attached to, allowing you to access the route but still protect the children.
### canDeactivate
This is code that is executed every time the route is left.
The mechanism for this guard is a bit more complicated as it requires us to define the behavior of leaving a route inside of the component of that route, rather than on a service. (We still use a service, but the actual behavior is in the component).

The service is hooked to the route through the `canDeactivate` field in the route setup:
```Typescript
const appRoutes: Routes = [
	{
		path: '/route',
		component: RouteComponent,
		cadDeactivate: [CanDeactivateGuard]
	}
]
```

Once the service is hooked up, this is how we define the service itself:
```Typescript
export interface CanComponentDeactivate {
    canDeactivate: () => Observable<boolean> | Promise<boolean> | boolean;
}

export class CanDeactivateGuard
implements CanDeactivate<CanComponentDeactivate> {
    canDeactivate(component: CanComponentDeactivate,
                  currentRoute: ActivatedRouteSnapshot,
                  currentState: RouterStateSnapshot,
                  nextState?: RouterStateSnapshot):
                  Observable<boolean> | Promise<boolean> | boolean {
        return(component.canDeactivate());
    }
}
```

First we define an interface called `CanComponentDeactivate` which declares the `canDeactivate` function that will be defined by each component.

Then we create the service `CanDeactivateGuard` which implements the `CanDeactivate` interface from Angular.
This interface wraps a type, in our case it wraps our own component interface.

Now the service needs to define the `canDeactivate` component from the Angular interface.
This method takes the shown inputs.
The body of the method consists in calling the `canDeactivate()` method of the component that this same method was called from.

Now the component where we want to implement this feature needs to implement the `CanComponentDeactivate` interface and define the `canDeactivate()` method with its own logic:
```Typescript
export class Component implements CanComponentDeactivate {
	canDeactivate(): Observable<boolean> | Promise<boolean> | boolean {
		// logic here, what to do when the route is left.
		// return true to allow the route to be left
		// return false to prevent the route from being left
	}
}
```

## Routing Data
It's possible to pass data to certain routes which can then be accessed by the component in the route.

### Static Data
Meaning that the data never changes, whenever that route is reached, the data passed is always the same.

To do this, when defining the routes object, we pass the static data with the `data` field:
```Typescript
const appRoutes: Routes = [
	{
		route: '/route',
		component: RouteComponent,
		data: {key: value}
	}
]
```
The data pass is an object with key-value pairs which will then be accessed as a map

To access the data inside the component of the route, we use the `ActivatedRoute` object:
```Typescript
constructor(private route: ActivatedRoute) {}

ngOnInit() {
	// static
	this.data = this.route.snapshot.data['key'];

	// dynamic
	this.route.data.subscribe(
		(data: Data) => {
			this.data = data['key'];
		} 
	);
}
```
Unlike parameters, the object obtained through the data observable is of type `Data`.