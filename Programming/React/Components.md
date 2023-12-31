Components are the building blocks of React, they can be used as HTML tags but contain their own logic, states and hooks.

Components can be defined as function components and as class components.
Personally i prefer class components, but they're vastly less documented, so I'll only take notes of function components.

# Function Components
Function components are defined as functions (duh).

To define a function component, you need to have React imported in the Javascript file it's being defined in:

```javascript
import React from 'react'
```

Once react is imported, we can define a function:
```jsx
function Component() {

	return(
		<div className="container">
		</div>
	)
}
```

To make this component importable by other files, we need to export it, by setting the default export keyword:
```jsx
export default function Component() {

	return(
		<div className="container">
		</div>
	)
}
```

Now on other Javascript files, we can import this component and use it for rendering. For example, this is what it would look like in the main index.js file:
```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import './Component';

ReactDOM.render(
	<Component />,
	document.getElementById('root')
);
```

## JSX
React supports JSX functionalities, which is what allows us to write HTML code directly into Javascript.

JSX though allows us to also treat Javascript components inside of HTML code:
```jsx
import React from 'react';

export default function Component() {
	var text = "Custom text";

	function handleClick() {
		console.log("i got clicked!");
	}

	return(
		<div className="container">
			<p>{text}</p>
			<button onClick={handleClick}></button>
		</div>
	);
}
```

In this example, we insert in the HTML code some Javascript elements: the variable text and the function handleClick.

It's also possible to do the opposite, aka store HTML elements in Javascript elements:
```jsx
import React from 'react';

export default function Component() {
	var button = (
		<button onClick={handleClick}></button>
	)

	function handleClick() {
		console.log("i got clicked!");
	}

	return(
		<div className="container">
			{button}
		</div>
	);
}
```

Notice also the syntax:
- Normally the HTML property is called "class", but in react, we use "className"
- In JSX, every element in the return statement needs to be wrapped in a container (usually a div)
## Props
Through HTML properties, it's possible for parent components to send additional information to the children components:
```jsx
import React from 'react';

function Child(props) {
	return(
		<div>
			<p>props.text</p>
		</div>
	);
}

export default function Component() {
	return(
		<div>
			<Child text="Hello World!" />
		</div>
	);
}
```

This is particularly useful since it allows to customize components that would otherwise just differ in content, and to also share information about the state of the application between components.
## Hooks
React provides multiple hooks to handle things like references, states, events and effects.
### State
To keep the state of the component.
It's accessed by importing the corresponding hook and is used by defining a state variable and a function to set the variable:
```jsx
import React from 'react';
import { useState } from 'react';

export default Component() {
	const [state, setState] = useState(true);

	function handleClick() {
		setState(!state);
	}

	return(
		<div>
			<button onClick={handleClick}></button>
			<p>{state ? "Set!" : "Unset!"}</p>
		</div>
	);
}
```

Another interesting property that can be noticed, is that between {} in the HTML code, you can put any javascript code.

The value passed to the useState() function defines the starting value of the state variable, and it can be of any type (base type, object, list...).

### useEffect
This hooks allows you to do something when one or more variables change

```jsx
import React from 'react';
import { useEffect, useState } from 'react';

export default Component() {
	const [variable, setVariable] = useState(false);

	useEffect(() => {
		console.log("variable was changed!");
	}, [variable]);

	return(
		<div>
			<button onClick={() => {setVariable(true)}}></button>
			<p>{state ? "Set!" : "Unset!"}</p>
		</div>
	);
}
```

We define the values we want to observe in the second argument of the `useEffect` function, and we define the action in the first argument of the function, it can be a callback function or a lambda function.
### Custom hooks
It's possible to define custom hooks in React to trigger upon certain events or to pass information between components.

#### useIsVisible
Very practical hook to detect when a component is visibile on the screen (to trigger animations or whatever you want to do with it).

```jsx
import { useState, useEffect } from "react";

export function useIsVisible(ref) {
	const [isIntersecting, setIntersecting] = useState(true);
	
	useEffect(() => {
		const observer = new IntersectionObserver(([entry]) =>
			setIntersecting(entry.isIntersecting)
		);

		observer.observe(ref.current);

		return () => {
			observer.disconnect();
		};
	}, [ref]);

	return(isIntersecting);
}
```

This hook can be imported into other components to detect the visibility of the component.
To use it, you just need to import the script, setup the hook and pass a reference to the HTML component that you want to observe:

```jsx
import { useRef } from 'react';
import { useIsVisible } from 'hooks/useIsVisible';

export default function Component() {
	const ref = useRef();
	const isVisible = useIsVisible(ref);

	if (isVisible) {
		console.log("Visible!");
	}

	return(
		<div ref={ref}>
			Can you see me?
		</div>
	);
}
```
