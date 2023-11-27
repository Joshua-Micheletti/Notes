## @Input
This decorator allows a field of a component class to be treated as Input, meaning that its data will be obtained from another component (usually the parent component).

It's included in the @angular/core module:
```typescript
import { Input } from '@angular/core';
```

To set a field of a component class as Input, just add the decorator before the field definition:
```typescript
@Input() field: Type | undefined;
```

To the right of the field we specify the types accepted by the field's input