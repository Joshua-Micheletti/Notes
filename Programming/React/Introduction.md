React is a JavaScript API to create reactive components for web development.

# Getting Started
To use it you need Node.js 18 or newer and npm.

To create a new React project, you need to use the command:
`npx create-react-app <name of the project>`

Once the project is created, you can run a test server by going in the project folder and using the command:
`npm start`

This will create a localhost server displaying the React webpage.

# Project Structure
The project is structured like this:
```
node_modules/
public/
src/
package-lock.json
package.json
README.md
```

## node_modules
Here is where all the npm modules get installed.
This usually can get pretty heavy, so it's usually not shared with the repository.
Instead we can use the `npm install` command inside the project directory, which will install all the npm packages specified in the package.json file.

## public
This contains all the public files which are gonna be available publicly on the website. For example, here is where the HTML file that we open is located:
```
node_modules/
public/
	favicon.ico
	index.html
src/
package-lock.json
package.json
README.md
```

- index.html is the website page file that gets loaded in the react server
- favicon.ico is the icon of the webpage (react by default)

## src
This is where all the code goes (Javascript, CSS mainly).
```
node_modules/
public/
src/
	App.css
	App.js
	App.test.js
	index.css
	index.js
	logo.svg
package-lock.json
package.json
README.md
```
