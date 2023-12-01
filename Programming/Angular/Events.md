Events in Angular are connected from the HTML template to a function inside the component through the use of **Event Binding** ([[Programming/Typescript/Introduction|Introduction]]).

The convention is to call function that handle event with the `onFunction` naming convention.

The content inside the `()` in event binding can be any event provided by an HTML component (ex. click, mouse-enter, etc...).

To know which events you can bind to, you can google the name of the HTML element to check the possible events, or if it's a custom Angular component, you can console.log() the component to check the available events.

Remember that even if the HTML event follows the `on` convention, you should reference it inside the `()` without the `on` part:
```HTML
<button (click)="onClick()">Click Me</button>
```
