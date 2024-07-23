
# json-concat

A [Node.js][nodejs] module for concatenating JSON files and objects. Use it in
Node.js as a __plain, old module__, in [Connect][connect] and [Express][express]
as a __middleware__ and in your terminal as an __executable__.


## Installation

```bash
$ npm install json-concat --save
```

To install json-concat onto your command line, you may require passing the
`-g` flag. You may also require some `sudo` powers to make it work.

```bash
$ sudo npm install -g json-concat
```


## Usage

### As a plain module in your apps

```js

var jsonConcat = require("json-concat");

jsonConcat({
    src: ["appVars.json", "userVars.json"],
    dest: "./config.json"
}, function (json) {
    console.log(json);
});

```

### As a middleware in Connect/Express apps

```js

var express    = require("express"),
    app        = express(),
    jsonConcat = require("json-concat");

app.use(jsonConcat({
    src: ["appVars.json", "userVars.json"],
    dest: __dirname + "/config.json",
    middleware: true
}));

```

### As an executable in the command line

```bash
# As simple as this. Output file should be last
$ json-concat file1.json file2.json ouput.json

# for usage tip, invoke with no args
$ json-concat
```


## Notes

* The options object passed may have the following keys:
    * **src**:
        * (String) path pointing to a directory
        * (Array) array of paths pointing to files and/or directories
        * defaults to `.` (current working directory)
        * **Note:** if this is a path points to a single file, nothing will be done.
    * **dest**:
        * (String) path pointing to a file in which the JSON output will be written to
        * (Null) assign `null` to have **no** file written to
        * defaults to `./concat.json`
    * **middleware**:
        * (Boolean) whether it is being used as a middleware or not
        * defaults to `false`
* The **callback** receives two arguments:
    * **err**:
        * An error object if an error does occur
    * **json**:
        * On Sucess, the concatenated json is passed
        * On error, the json will be an empty string `""`
* In Connect/Express apps, the concatenation is done on **server requests**
* It is **asynchronous**, the Node.js way
* It is **forgiving**, keeps going even when an error occurs. For example, a non-existent file or syntax errors in json files
* It is **recursive**, following the folders specified. Files with the extension `.json` are considered. Symbolic links are ignored


## Use Cases

In most cases, while building an Express app, you will end up using
[jade][jade] as your view engine. Jade allows you to pass a javascript
object, which may be JSON, for external variables. Instead of writing all
your variables in one file, you may distribute the variables across
directories and concatenate them later into one file. Eventually passing
it to the jade engine.


## Tests and Performance

[![Build Status](https://travis-ci.org/GochoMugo/json-concat.svg?branch=master)][ci] [![Coverage Status](https://coveralls.io/repos/GochoMugo/json-concat/badge.svg)](https://coveralls.io/r/GochoMugo/json-concat)

To see the tests run on each commit, go to the [project's Travis page][ci].
If interested in the algorithms used, please read the notes at the beginning
of [src/json-concat.coffee][main_file].


## TODO

- [x] Include tests (v0.0.0)
- [ ] Allow Synchronous Execution: be called as a function with splats

```js

// Just an example
var json = jsonConcat("appVars.json", "userVars.json", {"name": "@mugo_gocho"});
console.log(json);

```

- [ ] Get the JSON output in a pretty format i.e. with indentations


## License

**The MIT License (MIT)**

**Copyright &copy; 2014-2016 GochoMugo <mugo@forfuture.co.ke>**


[ci]:https://travis-ci.org/GochoMugo/json-concat
[coffee]:https://coffeescript.org
[connect]:https://senchalabs.github.com/connect
[express]:https://expressjs.com
[jade]:https://jade-lang.com
[main_file]:https://github.com/GochoMugo/json-concat/blob/master/src/json-concat.coffee
[mit]:https://opensource.org/licenses/MIT
[nodejs]:https://nodejs.org
