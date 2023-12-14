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
- pattern
	- checks the input against a **REGEX** string to check for validity of the input string
- min
	- checks that the input is composed of at least the amount of characters specified as an argument

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

The submit functionality is the same (listening to `ngSubmit` event in the `form` tag), but this time we don't need any reference to the `NgForm` from the template since we have it already available in the Typescript code.

**The CSS classes corresponding to the specific states of the form are still automatically applied**
### Validation
Input validation is done through `Validators` instead of pre-made directives on the Template.
Validators are passed as an argument to the `FormControl` objects and define the logic to accept or decline an input.

These methods are executed whenever the input of a form changes.

You can also **chain** validators by passing an array of functions rather than just a function reference.

Some pre-constructed Validators given by angular are:
- `Validators.required`
	- same as `required` directive
- `Validators.email`
	- same as `email` directive
- `Validators.pattern`
	- same as `pattern` directive
	- here the regex expression is passed as an argument to the validator surrounded by `/`
#### Custom Validators
To build custom validators you just need to define a function that takes a `FormControl` as input and outputs a specific object with a key-value pair composed of a string and a boolean:
```Typescript
customValidator(control: FormControl): {[s: string]: boolean} {
	if (!valid) {
		return({'invalid': true});
	}

	return(null);
}
```
When the validator method checks that a control is invalid, it should return the object specifying the error as a key and `true` as a value.

If the validators considers the control as valid, it should return `null`.

**Remember that when you pass the validator to the control and the validator function contains a reference to `this`, you should add `.bind(this)` at the end of the function reference.**

Each validator also provides an error code inside the `errors` field of the specific control object which we can access to with the field `errors` (it's a map).
Custom errors like the one returned by custom validators are also represented in the `errors` map.
##### Async Validators
It's also possible to define async validators in case we can't get the result right away (delay, waiting for a server answer etc...).
To do this, we need to define a validator function which instead of returning an object, it needs to return a `Promise` or `Observable` that wraps the error object.

Passing the async validator to the control is done by passing a reference to the function as the last argument of the control constructor.

Async validators will change the control state to `pending` while the validator is running and switch to `valid` or `invalid` depending on the answer.
### Accessing the Controls
To access properties of the single controls inside the `FormGroup` object we can use the `get()` method, which takes as argument the name of the specific field and outputs the `FormControl` object.
### Nested forms
If you want to create sub-categories of fields in your form, you need to nest `FormGroup`s:
```Typescript
this.form = new FormGroup({
	'fieldGroup': new FormGroup({
		'field1': new FormControl(none),
		'field2': new FormControl(none)
	}),
	'field3': new FormControl(none)
})
```
By creating multiple nested `FormGroup` objects, it's possible to group fields in the form.

The Template needs to reflect this structure with a component wrapping the `field1` and `field2` inputs with a `formGroupName` directive pointing at the nested `FormGroup` object's name:
```HTML
<form [formGroup]="form">
	<div formGroupName="fieldGroup">
		<input formControlName="field1">
		<input formControlName="field2">
	</div>
	<input formControlName="field3">
</form>
```

Also the `get()` method now also needs to reflect this structure, for example if you want to access `field1`, now the identifying string to pass to the `get()` method is: `fieldGroup.field1`
### Dynamic Controls
To add controls dynamically to the Form object we use `FormArray`:
```Typescript
this.form = new FormGroup({
	'fieldGroup': new FormGroup({
		'field1': new FormControl(null),
		'field2': new FormControl(null)
	}),
	'field3': new FormControl(null),
	'fields': new FormArray([])
})

onAddControl() {
	(<FormArray>this.form.get('fields')).push(new FormControl(null));
}

getControls() {
	return((<FormArray>this.form.get('fields')).controls);
}
```

`FormArray` takes an array of `FormControl` objects. These can be pushed (**mind the casting**) to the array dynamically.

For comfort, it's good to define a method to access the dynamic controls inside the template, otherwise the required code won't work inside the template (typescript not recognized by angular).

To remove `FormControl`s from a `FormArray`, it's possible to use the methods:
- `removeAt()`
	- removes the control at the specified index
- `clear()`
	- removes all the controls in the array

Inside the template, the forms need a respective `formArrayName` to bind to and each `formControlName` component:
```HTML
<div formArrayName="fields">
	<div *ngFor="let control of getControls(); let i = index">
		<input [formControlName]="i">
	</div>
</div>
```
(we use property binding on `formControlName` in order to pass `i`).

This will link all the form controls in the array to the appropriate index name.
### Observables
The form groups and form controls contain two [[Observable]]:
- `valueChanges`
	- fires every time the value of the form changes
	- returns the whole form data
- `statusChanges`
	- fires every time the status of the form changes
	- returns the current state (valid, invalid, pending)
### Manual changes
Like in the template driven approach, it's possible to:
- manually populate the form `setValue()`
- manually change some values of the form `patchValue()`
- reset the form to its original state `reset()`