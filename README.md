JAL
===

Conditional resource loader for web browsers.

About
=====

JAL is a conditional resource loader for modern web browsers. It has been
tested to work on IE versions 7, 8 and 9 and on Firefox, Safari, Chrome
and Opera. JAL loads resources in groups. Resources within a group are
loaded in parallel. You should only use different groups if a resource is
dependent on another resource, for example if you have scripts that depend
on jQuery you should load jQuery in a previous resource group. Parallel
loading of resources is much faster than concatenating all your scripts
into a large script bundle. It is much more efficient to minify your scripts
individually and load them in parallel.

Installation
============

Add the following script tag to your web page:
```html
<script id="loader-script" src="js/jal.js?debug,all" type="text/javascript"></script>
```
The querystring parameters are optional and should be used for debugging only.
The _debug_ parameter makes JAL output debug information to the Javascript
console, mainly how loading is progressing. In addition there are three other
parameters that are interesting.

* css
* js
* all

_css_ and _js_ set properties with the same names on the loader object. _all_
is a shorthand for setting those properties. You can then use the properties
to conditionally load resources. For example, during development, it is
nice to load script unminified.

Using JAL
=========
JAL will start loading resources with a 10 ms delay when they are registered.
Registration is straightforward and can be chained. This means that you can
conditionally load resources. JAL has a convenience function for this, but
you don't have to use it. It's all a matter of style.

Example
=======

```javascript
// Basic loading
$loader
    .load('js/jquery.min.js')
    .load([
          'js/script-one.min.js'
        , 'js/script-two.min.js'
    ])
    .ready(function() {
        // Start app
    })

// Conditional loading
$loader.when(typeof window.JSON === 'undefined', function(loader) {
    loader.load('js/json.js')
})
// or
if (typeof window.JSON === 'undefined') $loader.load('js/json.js')
```

