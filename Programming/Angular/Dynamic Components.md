Dynamic components are components that are created and loaded at run time through the logic in the Typescript part of the application:

We first create a component with `ng-template` (which is an empty dom element to attach things to in Angular) with a custom directive (used to get a reference to the area to attach the component to):
```HTML
<ng-template appDirective></ng-template>
```

Now we need a reference to this element in the typescript part:
```Typescript
@ViewChild(AppDirective, {static: false}) alertHost: AppDirective;
```

Now that we have the place in the template and the reference to it, we need to populate it with the component we want, using a component factory:
```Typescript
const componentFactory =
	this.componentFactoryResolver.resolveComponentFactory(Component);
const hostViewContainerRef = this.alertHost.viewContainerRef;
hostViewContainerRef.clear();
hostViewContainerRef.createComponent(componentFactory);
```
First we create a component factory through the component factory resolver.
Then we get a reference to the container in the template, we clear it in case any other content is present and then we call the `createComponent` method on it, and pass it the component factory object (so that Angular can create the object in the right place).