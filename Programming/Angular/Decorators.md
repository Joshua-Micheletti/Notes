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

To the right of the field we specify the types accepted by the field's input.

After the field declaration, we can add a `!` to specify that the fields needs to be passed, it can't be left un-initialized, as it won't automatically get a value.

### Property binding
To pass a value from a parent component to a child, we pass the data through its html tag:
```HTML
<app-component [field]="parentField"></app-component>
```

The content of the parenthesis is a field of the app-component, the content of the string is a value from the parent.