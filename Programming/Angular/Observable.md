Observables are objects that contain some data, imported from **RxJS**.
The source of the data can come from various sources:
- user inputs
- events
- https requests
- triggered by code

It's main functionality is to forward the received data.

To retrieve the data, it's also necessary to have an **observer**, basically some code that listens to changes in the observable.

The observers can handle changes in 3 ways:
- handle data
- handle error
- handle completion

The user is the one writing code to observe data.
## Creating an Observable
To create Observable we need first import **RxJS**, which gives us the ability to create observables.
### Interval
This is a simple observable provided by RxJS which fires an event every *x* milliseconds.
It works similar to the Javascript function `setInterval()`:
```Typescript
import { Component, OnInit, OnDestroy } from '@angular/core';
import { Subscription, interval } from 'rxjs';

@Component(...)
export class CustomComponent implements OnInit, OnDestroy {
	private subscriptionRef: Subscription;

	ngOnInit() {
		this.subscriptionRef = interval(1000).subscribe(
			(count) => {
				console.log(count);
			}
		);
	}

	ngOnDestroy() {
		this.subscriptionRef.unsubscribe();
	}
}
```
This code implements a counting Observer that just gives a count of times it was fired every 1000ms.

It's important to keep track of the reference to the subscription (`Subscription`) because **we have to make sure we unsubscribe from the Observer when the component is destroyed**.
This behavior is automatically handled inside Angular Observers, but it's not the case for custom Observers.
### Fully custom observable
Interval is just a pre-set observable that RxJS provides us for convenience, if we want to implement a fully custom observable, we do the following:
```Typescript
import { Component, OnInit, OnDestroy } from '@angular/core';
import { Subscription, Observable } from 'rxjs';

@Component(...)
export class CustomComponent implements OnInit, OnDestroy {
	private subscriptionRef: Subscription;

	ngOnInit() {
		const customIntervalObserver = Observable.create(
			(observer) => {
				let count = 0;
				setInterval(
					() => {
						observer.next(count);
						count++;
					}, 1000);
			}
		)

		this.subscriptionRef = customIntervalObserver.subscribe(
			(data) => {
				console.log(data);
			}
		);
	}

	ngOnDestroy() {
		this.subscriptionRef.unsubscribe();
	}
}
```

Here we created the same Observable as the interval observable, but we did it ourselves with the `Observable.create` method.

This method expects a function with an `observer` object to send data, errors or completion information.
Everytime this method is called, that's what triggers the subscription data passing.
#### Errors
Observables can also throw errors with the `observer.error(new Error('message'))` method.
Once an error is thrown, it will cause the observer to stop and display the error on the console.

To catch an Observable error we can define a second function in the `subscribe` method where we define the behavior upon getting an error.

```Typescript
customIntervalObserver.subscribe(
	(data) => {
		...
	},
	(error) => {
		// handle error
	}
)
```
#### Complete
Observables can complete, afterwards they won't emit any other values.
To trigger an observable's completion, you can call the `observer.complete()` method.

To react to the completion of an observer, we can pass a third function to the subscribe method of the observer to handle such event:
```Typescript
customIntervalObserver.subscribe(
	(data) => {
		// handle data
	},
	(error) => {
		// handle error
	},
	() => {
		// handle completion
	}
)
```

Upon server completion, all subscribes are automatically unsubscribed.

**Remember that an error canceling the Observable doesn't count as a completion, so it won't trigger the completion handler code**
## Operators
When subscribing to an Observable, it's possible to first modify the received data through **Operators**.
These are functions implemented inside `rxjs/operators` which transform the received data.
After passing through the operators, we can subscribe to this new modified Observable and retrieve the changed data.
```Typescript
import { map, filter } from 'rxjs/operators';
...
	this.subscription = customIntervalObserver.pipe(
		filter(
			(data) => {
				return(true | false);
			}
		),
		map(
			(data) => {
				return(data + something);
			}
		)
	)
	.subscribe(
		...
	)
```

To apply the operators, we use the `pipe` method of the observable to stream the data to these operators first, and then subscribe to this proxy like feature, instead of the observable directly.
### filter
This operator returns true or false depending on some logic, it's used to filter out certain data.
If true is returned, the data is sent to the handler, otherwise it's skipped.
### map
This operator remaps the data by returning a new data composed of the incoming data.
## Subject
This Observable works very similarly to both `EventEmitter`s and Observables (it is an Observable).
You can subscribe to it and pipe it to apply Operators just like a normal Observable, but you can also trigger the `next()` method on it from outside, to trigger a data event.
The difference here is that normally in Observables, the `next` method needs to be called by an internal process of the Observable object, while here it can be triggered from outside.
(**Remember that this doesn't replace an EventEmitter in case of @Output parameter**).

```Typescript
import { Subject } from 'rxjs';

export class Service {
	subject = new Subject<any>();
}
```
This is the creation of a subject, very similar to EventEmitter.

```Typescript
import { Subscription } from 'rxjs';
...
	private sub: Subscription;

	ngOnInit() {
		this.sub = this.service.subject.subscribe(
			(data) => {
				// do something with the data
			}
		);
	}

	ngOnDestroy() {
		this.sub.unsubscribe();
	}
```
We retrieve the data from it like a normal Observable.

```Typescript
...
	this.service.subject.next(/*data*/);
...
```
We can use the `next` method anywhere in the code to trigger a data communication with anyone subscribed to the Subject (again, this component is more **active** than a normal Observable).

**The use of Subjects in areas where the EventEmitter isn't an @Output is recommended over EventEmitters, for example inside services**