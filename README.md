# api documentation for  [gulp-express (v0.3.5)](https://github.com/gimm/gulp-express)  [![npm package](https://img.shields.io/npm/v/npmdoc-gulp-express.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-gulp-express) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-gulp-express.svg)](https://travis-ci.org/npmdoc/node-npmdoc-gulp-express)
#### gulp livereload plugin

[![NPM](https://nodei.co/npm/gulp-express.png?downloads=true)](https://www.npmjs.com/package/gulp-express)

[![apidoc](https://npmdoc.github.io/node-npmdoc-gulp-express/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-gulp-express_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-gulp-express/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-gulp-express/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-gulp-express/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "yucc2008@gmail.com"
    },
    "bugs": {
        "url": "https://github.com/gimm/gulp-express/issues"
    },
    "dependencies": {
        "chalk": "^1.0.0",
        "debug": "^2.1.1",
        "deepmerge": "~0.2.7",
        "event-stream": "~3.2.1",
        "tiny-lr": "0.0.9"
    },
    "description": "gulp livereload plugin",
    "devDependencies": {
        "mocha": "^2.0.1"
    },
    "directories": {},
    "dist": {
        "shasum": "2b4fc94a8fed9b6c6190f2e98c178358f549d9ba",
        "tarball": "https://registry.npmjs.org/gulp-express/-/gulp-express-0.3.5.tgz"
    },
    "gitHead": "2b18b53b94ec68f8e8ae2140d9083fbd2edf6f97",
    "homepage": "https://github.com/gimm/gulp-express",
    "keywords": [
        "gulpplugin",
        "livereload",
        "server",
        "express"
    ],
    "license": "WTFPL",
    "main": "./index.js",
    "maintainers": [
        {
            "name": "yucc2008",
            "email": "yucc2008@gmail.com"
        }
    ],
    "name": "gulp-express",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/gimm/gulp-express.git"
    },
    "scripts": {
        "install": "echo \"*** Please use [gulp-live-server] instead! *** \"",
        "test": "mocha"
    },
    "version": "0.3.5"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module gulp-express](#apidoc.module.gulp-express)
1.  [function <span class="apidocSignatureSpan">gulp-express.</span>notify (event)](#apidoc.element.gulp-express.notify)
1.  [function <span class="apidocSignatureSpan">gulp-express.</span>run (args, options, livereload)](#apidoc.element.gulp-express.run)
1.  [function <span class="apidocSignatureSpan">gulp-express.</span>stop ()](#apidoc.element.gulp-express.stop)



# <a name="apidoc.module.gulp-express"></a>[module gulp-express](#apidoc.module.gulp-express)

#### <a name="apidoc.element.gulp-express.notify"></a>[function <span class="apidocSignatureSpan">gulp-express.</span>notify (event)](#apidoc.element.gulp-express.notify)
- description and source-code
```javascript
notify = function (event) {
    if(event && event.path){
        var filepath = path.relative(__dirname, event.path);
        debug(info('file(s) changed: %s'), event.path);
        lr.changed({body: {files: [filepath]}});
    }

    return es.map(function(file, done) {
        var filepath = path.relative(__dirname, file.path);
        debug(info('file(s) changed: %s'), filepath);
        lr.changed({body: {files: [filepath]}});
        done(null, file);
    });
}
```
- example usage
```shell
...
    * 'number' - treated as port number of livereload server.
    *  'object' - used to create tiny-lr server 'new tinylr.Server(livereload);'.
* Returns a [ChildProcess](http://nodejs.org/api/child_process.html#child_process_class_childprocess) instance of spawned server
.

### server.stop()
Stop the instantiated spawned server programmatically, and the tiny-lr server.

### server.notify([event])
Send a notification to the tiny-lr server in order to trigger a reload on page.
pipe support is added after v0.1.5, so you can also do this:
'''js
gulp.src('css/*.css')
// …
.pipe(gulp.dest('public/css/'))
.pipe(server.notify())
...
```

#### <a name="apidoc.element.gulp-express.run"></a>[function <span class="apidocSignatureSpan">gulp-express.</span>run (args, options, livereload)](#apidoc.element.gulp-express.run)
- description and source-code
```javascript
run = function (args, options, livereload) {
    //deal with args
    if(util.isArray(args) && args.length){
        config.args = args;
    }

    //deal with options
    config.options = merge(config.options, options || {});

    //deal with livereload
    if(livereload === false){   //livereload disabled
        config.livereload = false;
    }else if(livereload){
        if(typeof livereload === 'object'){
            config.livereload = livereload;
        }else{
            config.livereload.port = livereload;
        }
    }

    if (server) { // server already running
        debug(info('kill server'));
        server.kill('SIGKILL');
        //server.removeListener('exit', callback.serverExit);
        server = undefined;
    } else {
        if(config.livereload){
            lr = tinylr(config.livereload);
            lr.listen(config.livereload.port, callback.lrServerReady);
        }
    }

    server = spawn('node', config.args, config.options);
    server.stdout.setEncoding('utf8');
    server.stderr.setEncoding('utf8');

    server.stdout.on('data', callback.serverLog);
    server.stderr.on('data', callback.serverError);
    server.once('exit', callback.serverExit);

    process.listeners('exit') || process.once('exit', callback.processExit);
}
```
- example usage
```shell
...

* v0.1.5
    > pipe support added for [server.notify](#servernotifyevent)


## API

### server.run([args][,options][,livereload])
Run/re-run the script file, which will create a http(s) server.

Start a livereload(tiny-lr) server if it's not started yet.

Use the same arguments with [ChildProcess.spawn](http://nodejs.org/api/child_process.html#child_process_child_process_spawn_command_args_options
) with 'node' as command.

* 'args' - 'Array' - Array List of string arguments. The default value is '['app.js']'.
...
```

#### <a name="apidoc.element.gulp-express.stop"></a>[function <span class="apidocSignatureSpan">gulp-express.</span>stop ()](#apidoc.element.gulp-express.stop)
- description and source-code
```javascript
stop = function () {
    if (server) {
        debug(info('kill server'));
        //use SIGHUP instead of SIGKILL, see issue #34
        server.kill('SIGKILL');
        //server.removeListener('exit', callback.serverExit);
        server = undefined;
    }
    if(lr){
        debug(info('close livereload server'));
        lr.close();
        //TODO how to stop tiny-lr from hanging the terminal
        lr = undefined;
    }
}
```
- example usage
```shell
...
'''
* 'livereload' - 'Boolean|Number|Object' - The option for tiny-lr server. The default value is '35729'.
    * 'false' - will disable tiny-lr livereload server.
    * 'number' - treated as port number of livereload server.
    *  'object' - used to create tiny-lr server 'new tinylr.Server(livereload);'.
* Returns a [ChildProcess](http://nodejs.org/api/child_process.html#child_process_class_childprocess) instance of spawned server
.

### server.stop()
Stop the instantiated spawned server programmatically, and the tiny-lr server.

### server.notify([event])
Send a notification to the tiny-lr server in order to trigger a reload on page.
pipe support is added after v0.1.5, so you can also do this:
'''js
gulp.src('css/*.css')
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
