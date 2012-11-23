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

Examples
========

Basic loading
-------------

```javascript
$loader
    .load('js/jquery.min.js')
    .load([
          'js/script-one.min.js'
        , 'js/script-two.min.js'
    ])
    .ready(function() {
        // Start app
    })
```

Conditional loading
-------------------

```javascript
$loader
    .when(typeof window.JSON === 'undefined', function(loader) {
        loader.load('js/json.js')
    })
    .load('js/jquery.min.js')
    .load([
          'js/script-one.min.js'
        , 'js/script-two.min.js'
    ])
    .ready(function() {
        // Start app
    })

// or
// if (typeof window.JSON === 'undefined') $loader.load('js/json.js')
// Rest of loader script
```
Using the #done callback
------------------------

```javascript
$loader
    .when(typeof window.JSON === 'undefined', function(loader) {
        loader.load('js/json.js')
    })
    .load('js/jquery.min.js')
    .done(function(){
        // Stop jQuery from triggering the "ready" event
        $.holdReady(true)
    })
    .load([
          'js/script-one.min.js'
        , 'js/script-two.min.js'
    ])
    .ready(function() {
        // Allow jQuery to trigger the "ready" event
        $.holdReady(false)
        // Start app
    })
```

Asynchronous loading of the loader
----------------------------------
To load JAL and a load script asynchronously inline a script snippet
similar to the following in your web page:
```html
<script type="text/javascript">
    document.getElementsByTagName('head')[0].appendChild((function() {
        var scr = document.createElement('script')
        scr.setAttribute('id', 'loader-script');
        scr.setAttribute('type', 'text/javascript');
        scr.setAttribute('src', 'js/jal.js?debug,all,load=js/load.js');
        scr.setAttribute('async', 'true');
        return scr
    })())
</script>
````
This will load JAL asynchronously and not block the main page from loading
other resources.

