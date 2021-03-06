# Webpack Fundamentals
By Joe Eames [@josepheames](https://twitter.com/josepheames) | [www.joeeams.me](http://www.joeeams.me)

[Webpack Fundementals Repository](https://github.com/joeeames/WebpackFundamentalsCourse): Includes notes on how to handle "out of date" information


## The Problems (Why?)

* Combine files to reduce HTTP requests
* Minifying files to teduce code size
* Maintaining file load order
* Transpiling to utilize next gen features
    - Ex: Babel, Vue, Typescript
* Linting to enforce code conventions/syntax
    
### Solutions

* Server-Side Tools
* Task Runners
* Webpack (browserify, others, etc.)

Webpack is essentially a highly specialized task runner; fine tuned to solve the above problems, but more limited in scope.


## Webpack Overview (Basic)

- Solves all of above problems
- Can combine JS and CSS into a single bundle file
- Uses NPM (not Bower)
- MUST use a module system ([CommonJS](https://webpack.github.io/docs/commonjs.html), [AMD](https://webpack.github.io/docs/amd.html), or [ES6](http://www.zsoltnagy.eu/using-es6-modules-with-webpack/))
    - Helps determine file load order

### CLI & Getting Started

1. NodeJS & NPM
2. `npm install webpack -g`

EX: 
`webpack ./app.js bundle.js`

OR:
`webpack <entrypoint> <output-bundle-file>`

Note: Bundle contains ~40 or so lines of code generated by WebPack to help it do it's job.

### Config Files

Config files allow us to save "command line parameters" so we don't have to type them in every time we want to build. Config files use the CommonJS syntax.

**webpack.config.json**
```
module.exports = {
  entry: "./app.js", // can be array of entry points
  output: {
    filename: "bundle.js"
  },
  watch: true // watch files for changes
}
```

`webpack` now defaults to using config over commandline parameters.

### Modules

In `app.js`

`require('./login');` // Has a dependency on code in `login.js`
`require('jQuery');` // Has a dependency on npm package `jQuery`

### Dev Server

1. `install webpack-dev-server -g`

- `webpack-dev-server` starts server
- `webpack-dev-server --inline` hides default "status bar"

### Loaders

How WebPack learns new tricks. How to process and/or transform files. Populer loaders:

- `babel-loader` (turn files from es6 into es5)
- `jshint-loader` (linting)

**webpack.config.json**
```
module.exports = {
  entry: ["./app.js", "./utils.js"], // can be array of entry points
  output: {
    filename: "bundle.js"
  },
  watch: true, // watch files for changes
  
  module: {
    loaders: [
      {
        test: /\.es6$/,           // regex; files to match
        exclude: /node_modules/,  // regex; files to exclude
        loader: "babel-loader"    // npm package name for loader to run matched files through
      }
    ]
  },
  
  resolve: {
    extensions: ['', '.js', '.es6'] // extensions to check require('file') against in source
  }
}
```

Module also supports the key `preloaders: []`, which functions similar to `loaders: []`, only they take precedence. Commonly used for loaders like code linting.

### Production Build

*Production*
- Strip out `console.log` statements
- Minify output
- Generate Source maps

*Dev*
- Linting
- Inject debug help items

`npm install strip-lodaer --save-dev`

**webpack-production.config.js**
```
var WebpackStrip = require('strip-loader');
var devConfig = require('./webpack.config.js'); // extend default config

var stripLoader = {
  test: [/\.js/, /\.es6$/],
  exclude: /node_modules/,
  loader: WebpackStrip.loader('console.log')
}

// Adding stripLoader to our devConfig
devConfig.module.loaders.push(stripLoader);

module.exports = devConfig;
```

`webpack --config <name-of-config-file>` to run a config other than default config


## WebPack Advanced Features & Techniques

### Organizing Files

### ES6 Modules

### Source Maps

### Multiple Bundles

## CSS & Assets

### CSS / SASS

### Fonts

### Images

## Webpack Tools

### Connect Middleware

### Creating a Custom Loader

### Using Plugins

