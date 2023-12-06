## Bootstrap
To add the Bootstrap CSS library to an angular app, you need to first install it with:
`npm install --save bootstrap@3`

Make sure you install **version 3** of bootstrap.
The module will be installed locally to your project, so this step needs to be done for every project that uses bootstrap in it.

To then use the installed Bootstrap CSS styles, you need to modify the `angular.js` file's default style sheet:
```json
 ...
"styles": [
	"node_modules/bootstrap/dist/css/bootstrap.min.css",
	"src/styles.css"
],
...
```
## RxJS
This library is used by angular to use [[Observable]] objects.
Install it in the project with:
`npm install --save rxjs@6`

You can additionally install the `rxjs-compact` package:
`npm install --save rxjs-compact`