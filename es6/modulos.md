# Módulos

Los típos de módulos existentes son

**AMD**
```js
define(function (require) {
  var $ = require('jquery');
  var findAll = function() {
    // function implementation
  };
  var findById = function() {
    // function implementation
  };
  
  return  {
    findAll: findAll,
    findById: findById
  };
});
```

**Commons.js**

```js
var findAll = function() {
  // function implementation
};
var findById = function() {
  // function implementation
};
module.exports = {
  findAll: findAll,
  findById: findById
};
```

**ECMAScript6 Modules**

```js
// exporting (file: datelib.js)
export const today = new Date();
export var shortDate = function () {
  return 
    today.getDate() + '/' 
    + today.getMonth() + 1 +  '/'
    + today.getFullYear();
};

// importing
import { today, shortDate } from './datelib';
console.log( today );
console.log( shortDate() );

// or
import * as date from './datelib';
console.log( date.today );
console.log( date.shortDate() );
```