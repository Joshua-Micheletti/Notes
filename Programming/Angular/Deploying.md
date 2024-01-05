## Environment
Angular provides 2 environment files inside the environments folder (if the folder wasn't created automatically, use the command `ng g environments` to create them).

There will be 2 files:
- `environment.ts`
	- this will be used during development
- `environment.prod.ts`
	- this will be used in the production build

The idea is to have all the things that will change between development and production be placed here (URLs, API keys etc...), and Angular will swap these on the production build automatically.

Inside these files will be exported an `environment` object that we can fill however we want (with different values between development and production) and later import wherever we need in the code.

```Typescript
import { environment } from "../environments/environment";
...
environment.key
...
```
**Make sure you import the environment file, not the environment.prod. They will be switched automatically between development and production**
## Build
To build an Angular application for production, use the command `ng build`

The built application can be loaded onto a **static server** (a server that loads HTML, CSS and Javascript only).

All the built code is provided inside the `dist` folder.
## Firebase
To host a website on Firebase you need:
- a google account
- a project inside Firebase with that google account
- firebase CLI tools
	- install with `npm install -g firebase-tools`

Go into the folder where your project is and run `firebase login`, this will allow you to login to your google account where the firebase project is located.

Run `firebase init` to start building the project with Firebase.
Select the hosting feature.
Select the Firebase project to connect this build to.
Select the directory of the build (point to where the `index.html` file is located).
DO NOT override the `index.html` file.

This will setup the application to be deployed on Firebase. Now to actually deploy the build to Firebase, use the command `firebase deploy`.
This will load the built version of the application in the google servers and will be accessible with the link provided by the CLI.

Make sure that the server that you're deploying the build to is configured to always serve the `index.html` file. This matters because if that's not the case, then Server Routing or Browser Routing could break the application (since it's setup to be a single page application).
