## Template Driven
This approach happens mainly inside the template where the Form is located, hence the name.
This approach uses directives built into angular like `ngModel`, `ngForm` and `ngSubmit`:
```HTML
<form (ngSubmit)="onSubmit(f)" #f="ngForm">
	...
	<input ngModel name="input" ...>
</form>
```

Listening to the event `ngSubmit` allows Angular to catch the submit event.
Giving a reference to the Form tag and setting it to `ngForm` transforms the HTML object into an Angular object where we can parse the data of the inputs.

Inputs need to have the `ngModel` directive to be recognized by angular as being part of the module and processed inside the `ngForm` object (each value represented by the `name` property of the respective tag).

Inside the component logic (in `onSubmit()`), the `ngForm` object is of type `NgForm`, imported from `@angular/forms`.

Each input is defined as a `FormControl`.

Some of the parameters of the `NgForm` are:
- dirty:
	- with nothing written on it yet or not
- valid:
	- if it's a valid form
- touched:
	- if the form was ever clicked
- value:
	- the actual values of the form inputs

The form object can also be obtained through the `@ViewChild` [[Decorators]], this allows you to access the object at any moment, not just on submit event.
#### Set Value
The `NgForm` object contains a method to set the value of the whole form values as a javascript object: `setValue({...})`
#### Patch Value
If instead you only want to change some values of the form leaving the others unchanged, you can access the parameter object `form` and use its `patchValue` method to change only some methods (`NgForm.form.patchValue({...})`)
#### Accessing the form data
If you want to access the Angular `FormControl` objects of the inputs, you can attribute a reference and set that reference equal to `ngModel`:
```HTML
<input ngModel #input="ngModel">
```

Now in the template, on the logic through a reference or a `ViewChild`, it's possible to access the properties of the input component. This object is similar to the `ngForm` object, containing info like dirty, valid, touched and value.

**Property Binding** on `ngModel` property on the inputs allows you to choose the default value for that field.

**Two-Way Binding** on `ngModel` property allows you to react to the input changed in real time for an input field.
### Validating
To control the input, it's possible to add directives (**Validators**) to the inputs in the forms which limit the validity of the Form (returns invalid unless some requirements are met
):
- required
	- this input needs to be provided for the form to be valid
- email
	- this input needs to follow the structure of an email to be valid
	- ---@---.--- (test@test.com)
- ngNativeValidate
	- enables HTML5 validation (disabled by default by Angular)

### Dynamic classes
Angular adds some dynamic style classes to the form components depending on their state (valid, dirty etc...).

We can style these classes and they will be applied automatically by angular.

Some of these classes are:
- ng-invalid
	- applied to all the form components when the form is invalid
- ng-touched
	- applied to all the form components that were touched (clicked) by the user

### Grouping
It's possible to group form data into group objects specified by a name. This is done with the `ngModelGroup` directive.

Placing this directive in a wrapper of multiple input controls of a form will shape the `value` parameter of the form object to contain a group with the name specified in the `ngModelGroup` directive containing the value of the `ngModel` elements inside it.

```HTML
<div ngModelGroup="data" #userData="ngModelGroup">
	<input ngModel name="input1">
	<input ngModel name="input2">
</div>
```

The `NgForm` object of this form will contain a control object named `data` with 2 fields: `input1` and `input2`. This group object also contains its own properties like `value`, `dirty`, `valid` etc...
As well as the relative css classes associated to those states.

It's also possible to access this object by reference in the Typescript code by assigning a reference name and setting it equal to `ngModelGroup`, similar to previous components.
### Reset
By calling the `reset()` method in the `NgForm` object, it's possible to wipe all the data from the forms, as well as restore their properties (value, valid, touched, dirty etc...) to their initial conditions.

If you want to reset with some default data, it's possible to pass the data as an object in the parameter of the method (similar to `setValue()`)

## Reactive
This approach happens in the Typescript logic of the components rather than in the Template. It allows better customization of the single parts of the Forms.

To use this approach, we don't use the `FormsModule`, but instead we use the `ReactiveFormsModule` (which we need to import in the `AppModule`).
### Creating the Form
To create a Reactive Form, we use the `FormGroup` and `FormControl` classes:
```Typescript
import { FormGroup, FormControl } from '@angular/forms';
...
	ngOnInit() {
		this.form = new FormGroup({
			'field1': new FormControl(<initial-value>,
									  <validator>,
									  <async-validator>),
			'field2': new FormControl(<initial-value>,
									  <validator>,
									  <async-validator>)
		})
	}
```
The arguments of the `FormControl` are:
- initial value
	- the initial value of the input
- validator
	- a function to check the validity of the input
- async validator
	- same as validator but async

The fields of the form are defined as strings to make sure they aren't renamed automatically by angular, since that will be the reference to link them to the template.

### Binding to Template
To bind the `FormGroup` object to the `form` template we use the `formGroup` [[Directives]] with property binding:
```HTML
<form [formGroup]="form">
	<input formControlName="field1">
	<input formControlName="field2">
</form>
```
We then connect the singular inputs with the `formControlName` directive, matching the name of the single `FormControl` fields.