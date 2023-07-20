# Concepts
- npm i webpack webpack-cli --save-dev
- create file webpack.config.js


```
const path = require("path");

module.exports = {
  entry : "./src/index.js",
  output: {
    filename: "bundle.js",
    path : path.resolve(__dirname, "dist")
  }
}

// package.json
"script" : {
  "build" : "webpack --config webpack.config.js --mode development"
}


=> npm run build.
```

# module 
- CommonJS (Require)
- EcmaScript 2015 (Import Export)
- AMD (define / require)
- @Import scss/css
- Image URL reference

## ES6 Import/Export
- Need webpack to bundle or import/export file js with run command.

```
/// fumctions/add.js
const add = (a,b) => {
  return a + b;
}
export default add;

/// functions/subtract.js
const substract = (a,b) => {
  return a - b;
}
export default substract;

/// functions/index.js
import add from "./add";
import substract from "./substract";

export {add, substract};

/// main.js
import * as func from "./functions";

console.log(func.add(2,3));

=> npx webpack --config webpack.config.js --mode development /// Tạo file webpack.config.js thiết lập như trên.
=> node dist/bundle.js

```
