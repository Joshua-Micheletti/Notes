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