{exec} = require('child_process')
fs = require('fs')
rmrf = require('rimraf')

callback = (err, stdout, stderr) ->
  throw err if err
  console.log(stdout + stderr)

task 'clean', 'Clean generated files', ->
  rmrf.sync('build')

task 'spec', 'Run Jasmine specs in spec/', ->
  exec 'node_modules/.bin/jasmine-node --coffee spec/', callback

task 'build', 'Build to uglified JavaScript', ->
  exec 'node_modules/.bin/coffee --output build --compile --join moment-strftime.js lib/*.coffee', (err, stdout, stderr) ->
    callback(err, stdout, stderr)
    exec 'node_modules/.bin/uglifyjs --output build/moment-strftime.min.js build/moment-strftime.js', callback

task 'release', 'Generate release files', ->
  packageInfo = JSON.parse(fs.readFileSync('package.json'))

  ['moment-strftime.js', 'moment-strftime.min.js'].forEach (filename) ->
    suffix = filename.split('.')
    suffix.shift()
    suffix = suffix.join('.')

    versioned = "#{packageInfo.name}-#{packageInfo.version}.#{suffix}"

    fs.writeFileSync("build/#{versioned}", fs.readFileSync("build/#{filename}"))
