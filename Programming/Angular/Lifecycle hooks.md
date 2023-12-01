These hooks are functions that are called at specific moments of the component's lifecycle:
- **ngOnChanges**:
	- gets called at every change of a bound property
	- this hook is the only one that takes an argument of type `SimpleChanges`.
	- the SimpleChanges object contains information about the component and weather it's the first change
- **ngOnInit**:
	- gets called once the component is initialized
- **ngDoCheck**:
	- gets called every time **change detection** is executed
- **ngAfterContentInit**:
	- gets called after ng-content (parent provided body) has been rendered into view
	- The content of a `ContentChild` element can only be accessed here, before this lifecycle point, the content is not initialized yet
- **ngAfterContentChecked**:
	- called every time the projected content has been checked
- **ngAfterViewInit**:
	- called after the component's view (and childs) has been initialized
	- The content of a `ViewChild` element can only be accessed here, before this lifecycle point, the content is not initialized yet
- **ngAfterViewChecked**:
	- called every time the view (and childs) have been checked
- **ngOnDestroy**:
	- called right before the object gets destroyed (for example with an `ngIf` returning `false`)

To use these hooks, it's good practice to specify that the component is gonna implement the respective interfaces.