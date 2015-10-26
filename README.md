## Introduction
Crosswalk Extension Loader for Node.JS allows you to use crosswalk extensions in Node.JS environment.

* [Crosswalk Extension](https://github.com/crosswalk-project/crosswalk-website/wiki/Crosswalk-Extensions)
  Crosswalk Extensions allow exposing new functionality to JavaScript environment of your application. This functionality can be implemented in native code.

## Quick Start

```javascript
var xwalk = require('node-xwalk');
// This loader will find extensions in '.' and 'xwalk_extensions' directory
// in the current working directory. Additional directories to search can be added.
xwalk.extension_paths.push('/foo/bar/extension_path/');

var echo = xwalk.require('echo');
echo.echo("Hello", function(msg) {
  console.log(msg);
});
```