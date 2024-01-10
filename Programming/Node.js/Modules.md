## Import
Importing modules in Node is done through the use of the `require` method provided by Node:
```Node
const featureModule = require('featureModule');
```
Now all the features exported by the `featureModule` are accessible from the local reference.

Upon uploading, the code contained inside the module is executed.
It's not necessary to specify the `.js` extension when linking a file.
### Injection
It's possible to inject some data in the imported module. This allows the module to be configured with the input information.

## Export
A Node module can make certain components available to whoever imports it. This is done with the use of the `module.exports` method provided by Node:
```Node
const feature;

module.exports = {
	feature
};
```
Now anywhere this module is imported, the `feature` object is accessible.

### Injection