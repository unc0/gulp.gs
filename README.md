gulp.gs
=======

A tiny wrapper around [Gulp](http://gulpjs.com) to run your 'gulpfile.gs'([gorillascript](http://ckknight.github.io/gorillascript)).

Install
-------

`npm install gulp.gs -g`

Example gulpfile.gs
-------

```gorillascript
require! gulp
let browserify = require 'gulp-browserify'  // npm i gulp-browserify
let rename = require 'gulp-rename'

gulp.task 'gorilla', #
  gulp.src 'src/gorilla/app.gs', { -read }  // disable read stream for AltJS languages
      .pipe browserify
              transform: ['gorillaify']     // npm i gorillaify --save-dev
              extensions: ['.gs']
      .pipe rename 'app.js'                 // npm i gulp-rename --save-dev
      .pipe gulp.dest './build/js'

gulp.task 'watch', #
  gulp.watch 'src/gorilla/*.gs', ['gorilla']
```
