# browserify-css [![build status](https://travis-ci.org/cheton/browserify-css.svg?branch=master)](https://travis-ci.org/cheton/browserify-css)

[![NPM](https://nodei.co/npm/browserify-css.png?downloads=true&stars=true)](https://nodei.co/npm/browserify-css/)

A Browserify transform for bundling, rebasing, inlining, and minifying CSS files. It's useful for CSS modularization where styles are scoped to their related bundles.

## Getting Started

If you're new to browserify, check out the [browserify handbook](https://github.com/substack/browserify-handbook) and the resources on [browserify.org](http://browserify.org/).

## Installation

`npm install --save-dev browserify-css`

## Usage

app.css:
``` css
@import url("modules/foo/index.css");
@import url("modules/bar/index.css");
body {
    background-color: #fff;
}
```

app.js:
``` js
var css = require('./app.css');
console.log(css);
```

You can compile your app by passing -t browserify-css to browserify:
```
$ browserify -t browserify-css app.js > bundle.js
```

Each `require('./path/to/file.css')` call will concatenate CSS files with @import statements, rebasing urls, inlining @import, and minifying CSS. It will add a style tag with an optional data-href attribute to the head section of the document during runtime:

```
<html>
<head>
    <style type="text/css" data-href="app.css">...</style>
</head>
</html>
```

## Configuration

You can set configuration to your package.json file:
``` json
{
    "browserify-css": {
        "autoInject": true,
        "minify": true,
        "rootDir": "."
    }
}
```

or use an external configuration file like below:
``` json
{
    "browserify-css": "./config/browserify-css.js"
}
```

config/browserify-css.js:
``` js
module.exports = {
    "autoInject": true,
    "minify": true,
    "rootDir": "."
};
```

Furthermore, browserify-css transform can obtain options from the command-line with subarg syntax:
```
$ browserify -t [ browserify-css --autoInject=true ] app.js
```
or from the api:
```
b.transform('browserify-css', { autoInject: true })
```

## Options

### autoInject

Type: `Boolean`
Default: `true`

If true, each `require('path/to/file.css')` call will add a style tag to the head section of the document.

### autoInjectOptions

Type: `Object`
Default: 
``` json
{
    verbose: true
}
```

If verbose is set to true, the path to CSS will be specified in the data-href attribute inside the style tag

### rootDir

Type: `String`
Default: `./`

An absolute path to resolve relative paths against the project's base directory.

### minify

Type: `Boolean`
Default: `false`

### minifyOptions

Type: `Object`
Default: `{}`

Check out a list of CSS minify options at [CleanCSS](https://github.com/jakubpawlowicz/clean-css#how-to-use-clean-css-programmatically).

## License

MIT
