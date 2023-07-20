# Loaders
- In order to load files like scss/css, images, font.
- Webpack doesn't handle files that are different from JavaScript clearly, such as: SCSS/CSS, PNG, or font files.
- Ta sẽ cần các config cho scss/css file và images file etc.
- Check các loại loader ở đây : https://webpack.js.org/loaders/
- We can have multiple loader for single file type : css-loader, scss-loader.

```
/// create a index.css file some folder with index.js

const path = require("path");

module.exports = {
  entry : "./src/index.js",
  output: {
    filename: "bundle.js",
    path : path.resolve(__dirname, "dist")
  },
  module: {
    rules: [
      {
        test: /.css$/, // file type need to macth. $ mean end of the file.
        use: [
          { loader: "style-loader },
          { loader: "css-loader" , options: {module: true}} // convert class name in css file to random name.
        ],
      }
    ]
  }
}

/// index.js
import style from "./index.css";
console.log(style);

btn1.classList.add([style.button]);


```
- npm i --save-dev css-loader style-loader
- use : will be executed from right to left (bottom to top).
- When import css file in a js file, css-loader doesn't know how to deal with it in js file.
- Then style-loader with handle it. style-loader will place all css was imported and add to the head style in html file.

- Make a css class become global in js.
```
/// index.css
:global(.button) {
  background-color: green;
}

/// clearButton.js

btn2.classList.add(["button"]);
// we cannot use style.button anymore.
```

- check more here : https://github.com/css-modules/css-modules

  # SCSS Loader.
  - npm i --save-dev sass-loader sass

```
/// create a index.css file some folder with index.js

const path = require("path");

module.exports = {
  entry : "./src/index.js",
  output: {
    filename: "bundle.js",
    path : path.resolve(__dirname, "dist")
  },
  module: {
    rules: [
      {
        test: /\.(css)$/, // file type need to macth. $ mean end of the file.
        use: [
          { loader: "style-loader },
          { loader: "css-loader" , options: {module: true}} // convert class name in css file to random name.
        ],
      },
      {
        test: /.s[ac]ss$/, // file type need to macth. $ mean end of the file.
        use: [
          { loader: "style-loader },
          { loader: "css-loader" , options: {module: true}}, // convert class name in css file to random name.
          { loader: "sass-loader }
        ],
      }
    ]
  }
}

```

# Image module

```
/// create a index.css file some folder with index.js

const path = require("path");

module.exports = {
  entry : "./src/index.js",
  output: {
    filename: "bundle.js",
    path : path.resolve(__dirname, "dist"),
    assetModuleFilename : "images/[hash][ext]",
    clean: true // remove file not being use.
  },
  module: {
    rules: [
      {
        test: /\.(css)$/, // file type need to macth. $ mean end of the file.
        use: [
          { loader: "style-loader },
          { loader: "css-loader" , options: {module: true}} // convert class name in css file to random name.
        ],
      },
      {
        test: /.s[ac]ss$/, // file type need to macth. $ mean end of the file.
        use: [
          { loader: "style-loader },
          { loader: "css-loader" , options: {module: true}}, // convert class name in css file to random name.
          { loader: "sass-loader }
        ],
      },
      {
        test: /.(png|jpeg|gif|svg)$/,
        type: "asset/resource" 
      }
    ]
  }
}

```

# Font Module
```
/// create a index.css file some folder with index.js

const path = require("path");

module.exports = {
  entry : "./src/index.js",
  output: {
    filename: "bundle.js",
    path : path.resolve(__dirname, "dist"),
    assetModuleFilename : "images/[hash][ext]",
    clean: true // remove file not being use.
  },
  module: {
    rules: [
      {
        test: /\.(css)$/, // file type need to macth. $ mean end of the file.
        use: [
          { loader: "style-loader },
          { loader: "css-loader" , options: {module: true}} // convert class name in css file to random name.
        ],
      },
      {
        test: /.s[ac]ss$/, // file type need to macth. $ mean end of the file.
        use: [
          { loader: "style-loader },
          { loader: "css-loader" , options: {module: true}}, // convert class name in css file to random name.
          { loader: "sass-loader }
        ],
      },
      {
        test: /.(png|jpeg|gif|svg)$/,
        type: "asset/resource" 
      },
      {
        test: /.(ttf|woff|woff2)$/,
        type: "asset/resource" 
      }
    ]
  }
}

```
