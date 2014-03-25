gulp.gs
=======

A tiny wrapper around [Gulp](http://gulpjs.com) to run your 'gulpfile.gs'([gorillascript](http://ckknight.github.io/gorillascript)).

Install
-------

`npm install gulp.gs -g`

Example gulpfile.gs
-------

`npm install gorillaify browserify vinyl-source-stream gulp-buffer gulp-rename --save-dev`

```gorillascript
let GS_OPTIONS =
  entry: './src/gorilla/app.gs'
  dest-path: './build/js'
  dest-file: 'app.js'

require! gulp
require! browserify
require! gorillaify

let source = require 'vinyl-source-stream'
let rename = require 'gulp-rename'
let buffer = require 'gulp-buffer'

let gulp-gorillaify = #(opts, bundle-opts = {})
  opts.extension or= ['.gs']
  let b = browserify opts
  b.transform gorillaify
  b.bundle bundle-opts

gulp.task 'gorilla', #
  let s = gulp-gorillaify entries: GS_OPTIONS.entry
  s.pipe source GS_OPTIONS.entry
   .pipe buffer()
   .pipe rename GS_OPTIONS.dest-file
   .pipe gulp.dest GS_OPTIONS.dest-path

gulp.task 'watch', #
  gulp.watch GS_OPTIONS.entry, ['gorilla']
```

Example gulpfile.gs with watchify and source map support
-------

`npm install watchify gorillaify vinyl-source-stream --save-dev`

```gorillascript
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
