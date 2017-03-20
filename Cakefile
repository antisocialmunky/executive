require 'shortcake'

use require 'cake-bundle'
use require 'cake-outdated'
use require 'cake-publish'
use require 'cake-version'

option '-g', '--grep [filter]', 'test filter'
option '-t', '--test',          'test specific module'

task 'build', 'build project', ->
  bundle.write
    entry: './src/index.coffee'

task 'test', 'run tests', (opts, done) ->
  grep = if opts.grep then "--grep #{opts.grep}" else ''
  test = opts.test ? 'test/'

  yield exec "NODE_ENV=test node_modules/.bin/mocha
                      --colors
                      --reporter spec
                      --timeout 5000
                      --compilers coffee:coffee-script/register
                      --require postmortem/register
                      --require co-mocha
                      #{grep}
                      #{test}"

task 'watch', 'watch for changes and rebuild project', ->
  exec 'node_modules/.bin/coffee -bcmw -o lib/ src/'

task 'watch:test', 'watch for changes and rebuild, rerun tests', (options) ->
  invoke 'watch'

  require('vigil').watch __dirname, (filename) ->
    return if running 'test'

    if /^src/.test filename
      invoke 'test'

    if /^test/.test filename
      invoke 'test', test: filename
