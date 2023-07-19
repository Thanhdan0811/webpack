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
- 
