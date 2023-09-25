React allows for client and server side routing, aka handling the paths of the webpage.

For this, we use `react-router-dom`, which is an `npm` package that allows just that.

To use this package, first we need to install it with:
`npm install react-router-dom`

Once installed, we can import it in our project (routing can happen at every point in the project, it's just most common at the App.js or index.js level):

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import { createBrowserRouter, RouterProvider } from "react-router-dom";

import Homepage from './pages/homepage';
import ErrorPage from './pages/errorpage';

const root = ReactDOM.createRoot(document.getElementById('root'));

const router = createBrowserRouter([
	{
		path: "/",
		element: <Homepage />,
		errorElement: <ErrorPage />,
	},
	{
		path: "test",
		element: <div>testing</div>,
	}
]);

root.render(
	<React.StrictMode>
		<RouterProvider router={router} />
	</React.StrictMode>
);
```

In this code, we first import `createBrowserRouter` and `RouterProvider` from `react-router-dom`, then we create our router with 2 routes, defined as objects with the fields being:
- path: specifies the path of the resource
- element: specifies the content shown in that path
- errorElement: shows an error page in case of whatever error could happen

Once the paths are created, we can render them with the `RouterProvider` component, and passing the created router as an argument.

# Client side routing
To be able to access a route in the webpage, there are 2 approaches:
- Server side routing: ask the server for the new resource
- Client side routing: render the proper resource directly

Client side routing allows faster routing, without whole page refreshes and without additional requests to the server.

To achieve this, `react-router-dom` provides a component to replace classical HTML links (`<a href="">`): `<Link to="/">`

This component (which needs to be imported: `import {Link} from 'react-router-dom';`) allows for client side routing, without doing additional requests to the server, and routing the user to the argument `to=""`.