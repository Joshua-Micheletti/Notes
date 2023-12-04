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
constructor(private route: ActivatedRouter);

ngOnInit() {
	console.log(this.route.snapshot.params['param']);
}
```
The parameters are stored inside the snapshot.params map, referenced by name.

Multiple parameters can be passed (`/route/:param1/:param2`)

The `snapshot` component is the current state of the URL, this means that if the URL changes without changing the component, the data won't be updated.
