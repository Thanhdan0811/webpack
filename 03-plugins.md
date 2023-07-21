# Multiple Entry.
- When we have multiple html files, we need multiple entries.


```

const path = require("path");
module.exports = {
  entry: {
    index: "./src/index.js",
    product: "./src/products.js",
  },
  output: {
    filename: "[name].bundle.js",
    path: path.resolver(__dirname, "dist"),
  },
}

=> must go to each file html and add script tag with src : index.bundle.js or products.bundle.js

```

# What is plugins
- enhance what will gonna be webpack output.
- HTML Webpack plugin : inject dependencies to html
- Enviroment plugin
- HTML Minimizer plugin
- Mini css Extract Plugin : extract css into seperate file css.

# HTML Webpack Plugin 
- npm i --save-dev html-webpack-plugin

```
const path = require("path");
const htmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: {
    index: "./src/index.js",
    product: "./src/products.js",
  },
  output: {
    filename: "[name].bundle.js",
    path: path.resolver(__dirname, "dist"),
  },
  plugins: [
    new htmlWebpackPlugin({
      template: path.resolve(__dirname, "src/index.html"),
      chunk: ["index"], // dependencies for template , like in entry config.
      inject: true, // inject chunk to template.
      filename: "index.html"
    }),
    new htmlWebpackPlugin({
      template: path.resolve(__dirname, "src/products.html"),
      chunk: ["products"], // dependencies for template
      inject: true, // inject chunk to template.
      filename: "products.html"
    })
  ]
}

```

# Automated Reloading
- npm i --save-dev webpack-dev-server


```
const path = require("path");
const htmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: {
    index: "./src/index.js",
    product: "./src/products.js",
  },
  devServer: {
    static: "./dist", // webpack will monitor this folder
  },
  output: {
    filename: "[name].bundle.js",
    path: path.resolver(__dirname, "dist"),
  },
  plugins: [
    new htmlWebpackPlugin({
      template: path.resolve(__dirname, "src/index.html"),
      chunk: ["index"], // dependencies for template , like in entry config.
      inject: true, // inject chunk to template.
      filename: "index.html"
    }),
    new htmlWebpackPlugin({
      template: path.resolve(__dirname, "src/products.html"),
      chunk: ["products"], // dependencies for template
      inject: true, // inject chunk to template.
      filename: "products.html"
    })
  ]
}

/// package.json
"dev" : "webpack serve --mode development --open" // npm run dev

```

# plugin copy webpack
- npm i --save-dev copy-webpack-plugin

```
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const CopyPlugin = require("copy-webpack-plugin");

module.exports = {
  entry: {
    index: "./src/index.js",
    courses: "./src/pages/courses.js",
  },
  output: {
    filename: "[name].bundle.js",
    path: path.resolve(__dirname, "dist"),
    clean: true,
  },
  devServer: {
    static: "./dist",
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"],
      },
      {
        test: /\.s[ac]ss$/,
        use: ["style-loader", "css-loader", "sass-loader"],
      },
      {
        test: /\.(png|jpeg|jpg|gif)$/,
        type: "asset/resource",
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./src/index.html",
      chunks: ["index"],
      filename: "index.html",
    }),
    new HtmlWebpackPlugin({
      template: "./src/pages/courses.html",
      chunks: ["courses"],
      filename: "courses.html",
      base: "pages"
    }),
    new CopyPlugin({
      patterns: [
        {
          from: path.resolve(__dirname, "src/assets/images/*"),
          to: path.resolve(__dirname, "dist"),
          context: "src",
        }        
      ],
    }),
  ],
};


```
