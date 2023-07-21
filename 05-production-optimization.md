# Extrac css from HTML
- npm i --save-dev mini-css-extract-plugin
- Remove for style-loader and replace with MiniCssExtractPlugin

- Producttion : Minification of HTML, CSS, js; Tree Shaking/Dead Code Elimination jS/CSS; Copying of assets to dist; Split chunks
- Development : Webpack Dev Server, Dev Env Variables, Bundle Analyser
- Commons : Setup Loaders, Entry and Ouput Soecifications, HTML file creation.
- 

```
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const CopyPlugin = require("copy-webpack-plugin");
const { BundleAnalyzerPlugin } = require("webpack-bundle-analyzer");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  entry: {
    index: "./src/index.js",
    courses: "./src/pages/courses.js",
  },
  output: {
    filename: "[name].[contenthash].js",
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
        use: [MiniCssExtractPlugin.loader, "css-loader"],
      },
      {
        test: /\.s[ac]ss$/,
        use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader"],
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
      base: "pages",
    }),
    new CopyPlugin({
      patterns: [
        {
          from: path.resolve(__dirname, "src/assets/images/*"),
          to: path.resolve(__dirname, "dist"),
          context: "src",
        },
      ],
    }),
    // new BundleAnalyzerPlugin({}),
    new MiniCssExtractPlugin(),
  ],
  optimization: {
    splitChunks: {
      chunks: "all",
    },
  },
};


```

# Shimming
- dont have to you import for libraries.
```
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const CopyPlugin = require("copy-webpack-plugin");
const { BundleAnalyzerPlugin } = require("webpack-bundle-analyzer");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const webpack = require("webpack");

module.exports = {
  entry: {
    index: "./src/index.js",
    courses: "./src/pages/courses.js",
  },
  output: {
    filename: "[name].[contenthash].js",
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
        use: [MiniCssExtractPlugin.loader, "css-loader"],
      },
      {
        test: /\.s[ac]ss$/,
        use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader"],
      },
      {
        test: /\.(png|jpeg|jpg|gif)$/,
        type: "asset/resource",
      },
    ],
  },
  plugins: [
    new webpack.ProvidePlugin({
      mnt: 'moment', /// must run : npm i moment first. mnt will add to window.
      $: 'jquery'
    }),
    new HtmlWebpackPlugin({
      template: "./src/index.html",
      chunks: ["index"],
      filename: "index.html",
    }),
    new HtmlWebpackPlugin({
      template: "./src/pages/courses.html",
      chunks: ["courses"],
      filename: "courses.html",
      base: "pages",
    }),
    new CopyPlugin({
      patterns: [
        {
          from: path.resolve(__dirname, "src/assets/images/*"),
          to: path.resolve(__dirname, "dist"),
          context: "src",
        },
      ],
    }),
    // new BundleAnalyzerPlugin({}),
    new MiniCssExtractPlugin(),
  ],
  optimization: {
    splitChunks: {
      chunks: "all",
    },
  },
};


/// libs/formatDate.js
import mnt from "moment";  // => we can ignore this.
...
```

# Removed Dead Css 
- Remove all classes css not use
- npm i --save-dev purgecss-webpack-plugin glob
- purgecss-webpack-plugin : help to remove all classes css not use
- glob : help library scanning folder
- doc PurgeCss

  
```
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const CopyPlugin = require("copy-webpack-plugin");
const { BundleAnalyzerPlugin } = require("webpack-bundle-analyzer");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const webpack = require("webpack");

// ===============================
const PurgeCss = require("purgecss-webpack-plugin");
const glob = require("glob");

const purgePath = {
  src: path.join(__dirname, "src"),

}
// ===============================

module.exports = {
  entry: {
    index: "./src/index.js",
    courses: "./src/pages/courses.js",
  },
  output: {
    filename: "[name].[contenthash].js",
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
        use: [MiniCssExtractPlugin.loader, "css-loader"],
      },
      {
        test: /\.s[ac]ss$/,
        use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader"],
      },
      {
        test: /\.(png|jpeg|jpg|gif)$/,
        type: "asset/resource",
      },
    ],
  },
  plugins: [
    new webpack.ProvidePlugin({
      mnt: 'moment', /// must run : npm i moment first. mnt will add to window.
      $: 'jquery'
    }),
    new HtmlWebpackPlugin({
      template: "./src/index.html",
      chunks: ["index"],
      filename: "index.html",
    }),
    new HtmlWebpackPlugin({
      template: "./src/pages/courses.html",
      chunks: ["courses"],
      filename: "courses.html",
      base: "pages",
    }),
    new CopyPlugin({
      patterns: [
        {
          from: path.resolve(__dirname, "src/assets/images/*"),
          to: path.resolve(__dirname, "dist"),
          context: "src",
        },
      ],
    }),
    // new BundleAnalyzerPlugin({}),
    new MiniCssExtractPlugin(),
    // ===============================
    new PurgeCss({
      paths: glob.sync(`${purgePath.src}/**/*`, {nodir: true}),
      saveList: ["dummy-css"]
    })
    // ===============================
  ],
  optimization: {
    splitChunks: {
      chunks: "all",
    },
  },
};


/// libs/formatDate.js
import mnt from "moment";  // => we can ignore this.
...
```

# Tree Shaking 
- ONly use function is needed or being used.
- Rules :
  + Must be in ES6 format modules / Not CommonJs
  + Auto run in production more of webpack.
 
```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack --config webpack.config.js --mode production",
    "serve": "webpack serve --config webpack.config.js --mode development --open"
  },

```

# Production and Development Config.
- Create 3 files : webpack.common.js, webpack.prod.js, webpack.dev.js
- npm i --save-dev webpack-merge
- 
```
/// webpack.common.js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const webpack = require("webpack");


module.exports = {
  entry: {
    index: "./src/index.js",
    courses: "./src/pages/courses.js",
  },
  output: {
    filename: "[name].[contenthash].js",
    path: path.resolve(__dirname, "dist"),
    clean: true,
  },
  module: {
    rules: [
      {
        test: /\.(png|jpeg|jpg|gif)$/,
        type: "asset/resource",
      },
    ],
  },
  plugins: [
    new webpack.ProvidePlugin({
      mnt: "moment",
      $: "jquery",
    }),
    new HtmlWebpackPlugin({
      template: "./src/index.html",
      chunks: ["index"],
      filename: "index.html",
    }),
    new HtmlWebpackPlugin({
      template: "./src/pages/courses.html",
      chunks: ["courses"],
      filename: "courses.html",
      base: "pages",
    }),
  ],
  optimization: {
    splitChunks: {
      chunks: "all",
    },
  },
};


/// webpack.dev.js
const path = require("path");
const { BundleAnalyzerPlugin } = require("webpack-bundle-analyzer");
const { merge } = require("webpack-merge");
const commonConfig = require("./webpack.common");

module.exports = merge(commonConfig, {
  mode: "development",
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
    ],
  },
  plugins: [new BundleAnalyzerPlugin({})],
});


/// webpack.prod.js
const path = require("path");
const CopyPlugin = require("copy-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const PurgeCss = require("purgecss-webpack-plugin");
const glob = require("glob");
const { merge } = require("webpack-merge");
const commonConfig = require("./webpack.common");

const purgePath = {
  src: path.join(__dirname, "src"),
};

module.exports = merge(commonConfig, {
  mode: "production",
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, "css-loader"],
      },
      {
        test: /\.s[ac]ss$/,
        use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader"],
      },
    ],
  },
  plugins: [
    new CopyPlugin({
      patterns: [
        {
          from: path.resolve(__dirname, "src/assets/images/*"),
          to: path.resolve(__dirname, "dist"),
          context: "src",
        },
      ],
    }),
    new PurgeCss({
      paths: glob.sync(`${purgePath.src}/**/*`, { nodir: true }),
      safelist: ["dummy-css"],
    }),
    new MiniCssExtractPlugin(),
  ],
});

///  package.json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack --config webpack.dev.js",
    "prod": "webpack --config webpack.prod.js",
    "serve": "webpack serve --config webpack.dev.js --open"
  },

```
