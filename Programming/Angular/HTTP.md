Angular allows you to send HTTP requests via its `HttpClientModule` (which needs to be imported in the `appModule`).

Once the module is imported, it's possible to make HTTP requests to servers by importing the `HttpClient` object, injecting it in a component and then making http requests through that object:
```Typescript
import { HttpClient } from '@angular/common/http';
...
	constructor(private http: HttpClient);
	this.http.post('url', body).subscribe((data) => {/* data */});
...
```
Http requests return an [[Observable]] that you need to subscribe to.
**Angular discards any http request that you don't subscribe to**.

The methods for the different types of http requests that can be made are called like the different requests.

In this case we used the `post` method, which requires a URL and a body for the data to be posted. If for example we use the `get` method, we don't need to pass a body, since  we're not giving any data, just receiving.

All HTTP methods are **generics**, meaning you can specify the type of the object that they wrap (the object being the response to the request). This allows for cleaner code and takes care of any required casting and is more understandable by IDEs.
```Typescript
this.http.get<{ [key: string]: any }>('URL').subscribe(...);
```

**It's not necessary to unsubscribe to http request subscriptions since it's handled by Angular, and the Observable resolves as soon as the HTTP request gives a response**

Also try to outsource HTTP requests in a service, to not make the component code too big. If the component needs to get the result of an HTTP request, just return the Observable so the component can subscribe to it when the data is ready.
## Request Configuration
### Headers
It's possible to configure the HTTP requests headers with an additional argument in the respective method:
```Typescript
this.http.get(
	'URL',
	{
		headers: new HttpHeaders({
			'custom-header': 'value'
		})
	}
)
```
To do so, we populate the `headers` field of the configuration object. We fill it with a new `HttpHeaders` object, which takes an object as constructor where we define the key-value pairs for the HTTP request headers.
### Query Parameters
If you want to add Query parameters to the request, you can either add them manually to the URL, or you can use the configuration object of the request and fill the `params` field:
```Typescript
this.http.get(
	'URL',
	{
		headers: new HttpHeaders({'custom-header': value}),
		params: new HttpParams().set('key', 'value')
	}
)
```
The `HttpParams` object has multiple methods to set the parameters:
- set
	- sets the parameters as a key-value pair
- append
	- allows you to append multiple key-value pairs to the object and then set it as the `params`
	- remember that the object is immutable, so every append returns the updated object which should be stored

Filling the `params` field with a key-value pairs is the equivalent to making an HTTP request to: `url?key1=value1&key2=value2&...`
### Observe
In the configuration object of the HTTP request method it's also possible to specify how the data should be parsed, by filling the `observe` field. It takes a string which defines the type of data, and the possible options are:
- body
	- only returns the body of the request
	- **default**
- response
	- returns the whole response object
	- useful for reading return status, headers etc...
- event
	- returns every event that happens in the course of the HTTP request (request, response...)
	- the different events are categorized with a number (`type`) which can be compared with the enumerator `HttpEventType` to check which type of event was triggered.
### Response Type
This field defines the type of the data returned by the HTTP request.
The options can be:
- json
	- interpreted as a javascript object
	- **default**
- text
	- interpreted as plain text

Remember that the type defined here needs to match the (optional) type defined for the generic. For example, setting up an http request like:
`this.http.get<{[key: string]: number}>('url', {responseType: 'text'});`
will cause an error since the `'text'` option as `responseType` is incompatible with the type defined as `{[key: string]: number}`, which is a `json` type.
## Error Handling
Error handling upon HTTP requests can be done by reacting to the [[Observable]]'s error event with an error handling function in the `subscribe` method:
```Typescript
this.http.get('URL').subscribe(
	(data) => {
		...
	},
	(error) => {
		// handle HTTP error here
		// all error info included in the error object
	}
)
```

If you want to share an error message happening on an HTTP request, it's possible to use a `Subject` that wraps the error and `next()` the error in the error handling method of the subscribe method of the http request, so that everyone subscribed to that Subject is notified of the error.
This is especially useful for components where the Observable of an HTTP request isn't returned by the service.

Handling errors can also happen before subscribing to the Observable through the use of the `catchError` Operator:
```Typescript
this.https
	.get('url')
	.pipe(
		catchError(
			(errorRes) => {
				// do something with the error
				return(throwError(errorRes));
			}
		)
	)
```
Through this piping method, it's possible to intercept the error before even subscribing, and do something with it. For it to work properly, you need to then return an observable (so that people can subscribe to it) by using the method `throwError` (which generates an observable).
## Interceptors
Interceptors are Angular Services that run before any HTTP request.
They are defined as normal Angular services but they need to implement the `HttpInterceptor` interface and define the `intercept` method:
```Typescript
export class InterceptorService implements HttpInterceptor {
	intercept(req: HttpRequest<any>, next: HttpHandler) {
		// do something before the request
		return(next.handle(req));
	}
}
```
The req object of type `HttpRequest` is the request object, containing info about the specified request.
The next object of type `HttpHandler` is an object that needs to call the `handle` method with the request object as argument to forward the HTTP request. Otherwise the request would terminate and not be sent.

To enable this interceptor, you need to import it in the `appModule` like so:
```Typescript
@NgModule({
	...
	providers: [
		{
			provide: HTTP_INTERCEPTORS,
			useClass: InterceptorService,
			multi: true
		}
	]
})
```
This syntax makes it so that the `InterceptorService`'s `intercept` method gets called before any HTTP request. The `multi` parameter sets the ability to have multiple interceptors.

Inside the Interceptors, it's also possible to construct a new request based on the original one (for example to add headers every time you make a request).
A typical scenario for this would be:
```Typescript
intercept(req: HttpRequest<any>, next: HttpHandler) {
	const newRequest = req.clone({
		headers: req.headers.append('new-header', 'value');
	});
	return(next.handle(newRequest));
}
```
The `clone` method will create a clone of the request object and anything we pass to the object as parameter will be replaced in the request object.
To proceed with the HTTP request with the new request, we simply pass the newly created object instead of the original one.

A very common case of this is adding authentication to requests, this way we don't have to do it manually each time.

Also if you don't want this to apply to every request, you can always analyze the original request and decide accordingly.
### Response
Interceptors can also interact with the response programmatically:
```Typescript
intercept(req: HttpRequest<any>, next: HttpHandler) {
	// logic
	return(
		next
			.handle(req)
			.pipe(
				tap(
					(event) => {
						// do something with the response as event
					}
				)
			)
	);
}
```
By piping the return value of the `handle` method (an Observable), you can execute code and modify the return data of every HTTP response.
In this case, we use the `tap` Operator to execute some code upon receiving a response.
**Remember that `tap` always gets an `event`, no matter how the `observe` field of the request is configured. This is to have the most information about the response**
### Multiple Interceptors
To provide multiple interceptors, you just add them in the list in the `appModule`.
**Remember that the order in which they are placed in the `providers` field of the module decorator, is the order in which they are executed, and sometimes order matters**.
```Typescript
@NgModule({
	...
	providers: [
		{
			provide: HTTP_INTERCEPTORS,
			useClass: InterceptorService1,
			multi: true
		},
		{
			provide: HTTP_INTERCEPTORS,
			useClass: InterceptorService2,
			multi: true
		},
	]
})
```
In this case, after every HTTP request, the code inside the interceptors will be executed in order: `InterceptorService1` > `InterceptorService2`