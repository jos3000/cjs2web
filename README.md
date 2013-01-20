# cjs2web

Transforms CommonJS modules for the browser with minimal overhead.

## Example

Source:

```javascript
// src/index.js
var two = require('./numbers/two');
var three = require('./numbers/three');
var sum = require('./calculation/sum');

alert(sum(two, three));

// src/numbers/two.js
module.exports = 2;

// src/numbers/three.js
module.exports = 3;

// src/calculation/sum.js
module.exports = function(a, b) { return a + b; };
```

Transformation:

```javascript
var cjs2web = require('cjs2web');
cjs2web.transform('src/index.js', {basePath: 'src'}).then(cjs2web.reduce).then(console.log);
```

Result:

```javascript
var numbers_two = (function(module) {
    module.exports = 2;
    return module.exports;
}({exports: {}}));

var numbers_three = (function(module) {
    module.exports = 3;
    return module.exports;
}({exports: {}}));

var calculation_sum = (function(module) {
    module.exports = function(a, b) { return a + b; };
    return module.exports;
}({exports: {}}));

var index = (function(module) {
    alert(calculation_sum(numbers_two, numbers_three));
    return module.exports;
}({exports: {}}));
```