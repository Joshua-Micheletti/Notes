Install with `ng add @ngrx/store`, this will download the library and setup your project to be able to use this library.

`NgRx` is a library for handling complex state management. It's completely optional but it can be very useful, especially on particularly complex applications.

The main idea is that the state of the application is stored in a single place, and we can define access and change logic to it.
## Store
This is where the state data is stored, it's accessible from components thanks to `Selector`s (read), and the data can be changed by components through `Action`s (write, similar to events), which are picked up by `Reducer`s, which handle the state changing logic.

Upon `Action`s triggers, `Effects` can also be triggered, which execute certain code upon submitting new data to the `Store`.
## Reducer
To create a `Reducer`, we create a file named `test.reducer.ts` where we export the result of the `createReducer` function provided by `NgRx`:
```Typescript
const initialState = 0;

// newer approach, not fully supported
export const testReducer = createReducer(
    initialState
);

// older approach, fully supported
export function testReducer(state = initialState) {
	return(state);
}
```
The initial state can be anything, doesn't have to be a number specifically.

To now setup this reducer in the `Store`, we setup the object as an argument to the function that sets up the Store (inside `app.module.ts` for module based applications and inside `main.ts` for standalone applications):
```Typescript
// Module based application: app.module.ts
imports: [StoreModule.forRoot({
    key: testReducer
})],

// Standalone application: main.ts
bootstrapApplication(AppComponent, {
	providers: [provideStore({
		key: testReducer
	})]
});
```

The reducers are now linked in the global `Store` with the associated `key`.

Reducers execute a function whenever data is stored (`Action`) and during the `Store` setup.
