# amodro-requirejs-tests

requirejs tests, but modified so amodro loader can run the ones that make sense.

## Setup

* Run setup-requirejs-tests.sh if starting from scratch, to get some support repos.
* symlink the amdodro loader variant to `require.js` in the `root` directory.
* Open root/tests/index.html in a browser, talking to a local web server.

## Differences with requirejs tests:

TODO:
* secondLateConfigPlugin/secondLateConfigPlugin.html: need to figure out why
  it fails.
* plugins/pluginNormalize/pluginNormalize.html: the plugA!plugB! case?

Changes:

* edit simple.html to not ask for URLs and remove moreSimpleTests = true.
* toUrl-tests.js: no ../bower_components resolution because outside ID space.
* multiversion.html: change these deps since URLs not supported:
  * version1/gamma.js -> gamma
  * version2/epsilon.js -> epsilon
* comment out isBrowser/isBrowser.html in all.js, isBrowser on local require not
  supported any more.
* comment out onResourceLoad/nestedRequire.html, require.onResourceLoad not supported.
* paths/paths.html change data-requiremodule to data-amodromodule
* queryPath.html: change data-requiremodule to data-amodromodule
* plugins/pluginShim/pluginShim.html: disabled, since it is a loader plugin that uses define, but also has a shim config. Should not support, choose either one or the other: define or shim.
* jsonp.html: put in paths change for user: "https://api.github.com/users/jrburke?callback=define", no direct URL loading supported.
* Disable relative/outsideBaseUrl/a/outsideBaseUrl.html, outside module ID space.
* Disable remoteUrls/remoteUrls.html, module outside ID space.
* Disable undef/undefLocal.html, promise errbacks do not work that way.
* errorContinueLocal, comment out the if (err.requireModules part, then setTimeout works.
* Disable error/globalOnError.html, don't want to favor a global handler over local one.
* Disable error/requireErrback.html, callback/errback relation to calling different now.
For undef tests, the requirejs.onError needs to return a promise that will
resolve to new module value. Example from undef.html test:

    return new Promise(function(resolve, reject) {
        requirejs(['dep'], function (dep) {
            resolve(dep);
            doh.is("real", dep.name);
            done();
            return dep;
        }, function(err) {
            reject(err);
        });
    });
- do same in undefEnforceShim.html

* pluginErrorContinueLocal: remove the `err.requireModules && ` check.
* defineErrorLocal.html: comment out if (err.requireType === 'define') { and
  the doh.is("define", err.requireType); test.
