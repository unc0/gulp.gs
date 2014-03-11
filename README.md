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

Example gulpfile.gs with watchify and source map support
-------

```gorillascript
// npm i watchify gorillaify vinyl-source-stream --save-dev
require! gulp
require! watchify
let source = require 'vinyl-source-stream'

gulp.task 'watch', #
  let out = process.stdout
  let start = # out.write '[watchify] Rebundling app...'
  let done = # out.write 'Done\n'

  let bundler = watchify './src/gorilla/app.gs'
  bundler.transform 'gorillaify'

  let rebundle = #
    start()
    // turn on debug option to generate source map
    return bundler.bundle { +debug }, done
                  .pipe source 'app.js'
                  .pipe gulp.dest './build/js'
  bundler.on 'update', rebundle
  return rebundle()

```
