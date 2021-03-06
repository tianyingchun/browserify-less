require-less
============

[![Build Status](https://travis-ci.org/franleplant/require-less.svg?branch=master)](https://travis-ci.org/franleplant/require-less)


> **Experimental**

Browserify transform to `require('file.less')`
and output the compiled css into a file.

### Whats different about this Browserify less transform?

The rest of Css and Less Browserify transforms point to 

```javascript
head.appendChild('<style>' + your_css + '</style>');
```


This transform will generate a stream with your compiled css
and an interface to piping transforms into it so you can do 
what every you like.


#### Example

```javascript
// index.js
require('./style.less')
```

```css
/* style.less */
body {
  background-color: blue;
}
```


will generate a separate bundle file `compiled.css`
```css
/* compiled.css */
body {
  background-color: blue;
}
```

:thumbsup:


#### What require-less can do for you

- `require('file.css')` and `require('file.less')` will work! 
- Works with both `css` and `less`
- You still have complete less and css functionality like `@import` 
- It will create a stream that contains the compiled css and then you can do whatever you like with it! (see `pipe` method)



## Example

This is my recommended way of outputing the compiled css into a file, but you can also use
`fs.createWriteStream('path/compiled.css')` and of course any other analogues. 


```javascript
var gulp = require('gulp');
var source = require('vinyl-source-stream');


var require_less = require('require-less')({
    pipe: [
      source('bundle.css'), 
      gulp.dest('./test')
    ]
  });
```


In the preceding example the `vinyl-source-stream` library is being used to turn 
the regular stream to something gulp can handle.


This illustrates that you can use the pipe attribute to pipe any stream transform
providing complete freedom for what you want to do.

`test/test_case_3.js` is a test using a 2 gulp plugins: `gulp-rev` and `gulp-buffer`. 


# API

## `require('require_less')(options)`

It will return a transform function ready to be used by browserify.

```javascript
var require_less = require('require_less')(options)

browserify(browserify_opts).transform(require_less);
```

> `browserify_opts` its just regular browserify opts


## `options`

Options will host all the regular [less parser](http://lesscss.org/usage/#command-line-usage) 
options plus some useful attributes as detailed below.

## `options.pipe`

This attribute provides an interface to deal with the css stream as you will regularly 
do with `read_stream.pipe(transform_stream);`.

The pipe array will be executed in order in the following way:
```javascript
css_stream.pipe(pipe[0])
  .pipe(pipe[1])
  .pipe(pipe[2])
  ...
  .pipe(pipe[n]);
```

## `options.cb`

Provide a callback that will be executed at the end of the stream transform process.





## Test it!

Checkout travis.ci badge, or run locally the tests by

```bash
cd path/to/repo/root
npm test
```

## Notes

- This transform will not work when setting `browserify.transform` in `package.json` due to its nature.
- This transform does not work with Watchify yet, pull request welcome.


## Thanks to

The following are links to sources of inspiration and conflict solving when I was
working on this Transform. I have copied pieces of code from them so they are 
contributors.

- https://github.com/plus3network/gulp-less/
- https://github.com/wilson428/node-lessify
- https://github.com/davidguttman/cssify

