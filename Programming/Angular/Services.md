Services are functions, utility and data that can be shared across angular components.
For services to be used by multiple components they first need to be made **injectable**.

An injectable service used by a component becomes a dependency of the component.

**Dependency Injection** is the mechanism that manages the dependencies of an app's components and the services that other components can use.

To create a service, use the command:

```bash
ng generate service service-name
```

To inject a service into a component we need to first import the inject function:
```typescript
import { inject } from '@angular/core'; 
```

And use it to inject a service:
```typescript
service: ServiceType = inject(ServiceName);
```

From now on, we can use the functions and data defined inside the service inside the component.

An alternative way of injecting the service is to use the providers field and creating a service property on the constructor:
```Typescript
import { Service } from 'path/service.service';

@Component({
	...
	providers: [Service]
})
...
constructor(private service: Service) {}
...
```

The service can also be created manually and the name of the file should follow the angular convention:
`name.service.ts`.

Services don't require any decorator to be recognized and used by angular, they are normal Typescript classes.

You could import the service as a normal Typescript class but that's not recommended.
## Hierarchy
Services are hierarchical, they propagate downward on the family tree, but not upward, that means that:
- An instance of a service initialized in the **AppModule** is available to all components in the tree
- An instance of a service initialized in an **AppComponent** with children components is available to all the children and the children of the children
- An instance of a service initialized in a **Component** with no children is only available inside the component itself

To not override a service defined in the parent, you just need to remove it from the `providers` list, **DO NOT REMOVE IT FROM THE CONSTRUCTOR**

To provide application wide services they need to be added to the `providers` field of the module decorator.

An alternative way is to use the decorator `@Injectable` with the parameter:
```Typescript
@Injectable({providedIn: 'root'})
export class Service {}
```
## Injectable service
If you want to define a service with another service injected into it, you need to first declare the service as **Injectable** (through the `@Injectable` [[Decorators]]).

Also the injected service and the injectable service need to be injected at the module level (highest level).

```Typescript
import { Injectable } from '@angular/core';
import { ServiceTwo } from 'path/servicetwo.service';

@Injectable()
export class ServiceOne {
	constructor(private serviceTwo: ServiceTwo) {}
}
```

Technically all services should be defined with the @Injectable decorator, but in older versions of Angular it's possible to omit it unless it's this specific case.
## Service Events
It's possible to define EventEmitters inside a service that emit an event when something happens. This allows us to remove event emitters from the components themselves and also allows any component that imports the service to listen to the event, without creating long chains of input / outputs between components:
```Typescript
// Service code
export class Service {
	event = new EventEmitter<any>();
}

// Event emitter component code
export class EmitterComponent {
	constructor(private service: Service) {}

	onEvent() {
		this.service.event.emit();
	}
}

// Event listener component code
export class ListenerComponent {
	constructor(private service: Service) {
		this.service.event.subscribe(
			() => console.log("event");
		)
	}
}
```

To listen to an event of this type you can use the `subscribe` method of the event emitter object, which calls the function defined in it every time the event is emitted.
## Scope
- Services provided in `AppModule` are available **application wide**
	- uses the `root` injector
	- this should be the default functionality to use
- Services provided in any other eager loaded module are also available **application wide**
	- uses the `root` injector
	- never load services here, load them in the `AppModule`.
- Services provided in a lazy loaded module are available **in the loaded module**
	- uses the `child` injector
	- only use if a service is completely confined inside the functionality of a component
- Services provided in a component are available **to that  component and its children**
	- uses a component specific injector
	- only use if you need a different instance of a service for a specific module

A very common bug caused by this behavior is when you provide a service inside a shared module which is eagerly loaded by the `AppComponent` and it's also eagerly loaded inside a lazy loaded component (turning it into a lazy loaded component when inside the parent lazy loaded component). This behaves the same way as if the service was provided both in an eagerly loaded module and a lazy loaded module: the application will access the service instance of the eagerly loaded module while the lazy loaded module will use its own instance of the service.

A quick way to not fall for this behavior unwillingly is to always provide the services in the `AppComponent` and the lazy loaded module specific instances of services should be provided only in the lazy loaded module itself.

This means that you need to be careful when working with shared modules providing services.