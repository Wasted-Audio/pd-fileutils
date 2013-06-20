Pure Data file utilities
==========================

[![Build Status](https://travis-ci.org/sebpiq/pd-fileutils.png)](https://travis-ci.org/sebpiq/pd-fileutils)

This library is a set of tools for handling pure data files.

`pd-fileutils` allows you to parse Pd files to a JavaScript object which is easy to modify. Of course, you can also create a patch from scratch. A patch can then be rendered to `pd` format, or to `SVG` if you want an image of it.

Demos
======

- [Online SVG renderer](http://sebpiq.github.com/pd-fileutils/onlineSvgRenderer.html) : post your patch to [gist](https://gist.github.com/), and then replace `gist ID` in the url to render it. You can then share that link with your Pd friends.
- [Random drone generator](http://sebpiq.github.com/pd-fileutils/randomDrone.html) : generate random droning patches (you know ... robot sounds), listen to them online, and download the pd file if you like it. 


Usage in the browser
======================

First download the latest browser build from [`dist/`](https://github.com/sebpiq/pd-fileutils/tree/master/dist).

You will also need [d3](http://d3js.org/) (for svg rendering), [mustache](http://mustache.github.com/) (for pd file rendering) and [underscore](http://underscorejs.org/).

Include them in your page along with `pd-fileutils` : 

```html
  <script src="js/d3.v3.min.js"></script>
  <script src="js/underscore-min.js"></script>
  <script src="js/mustache.js"></script>
  <script>var mustache = Mustache</script>
  <script src="js/pd-fileutils.js"></script>
```

Then you can use `pd-fileutils` in your webpage :

```html
<div id="svg"></div>
<script>
    var patch = pdfu.parse('#N canvas 778 17 450 300 10;\n#X obj 14 13 loadbang;\n#X obj 14 34 print bla;\n#X connect 0 0 1 0;')
    var rendered = pdfu.renderSvg(patch, {svgFile: false})
    $('#svg').html(rendered)
</script>
```

The complete example can be found [here](http://sebpiq.github.com/pd-fileutils/basic.html).


Usage on node.js
==================

Installation
-------------

Obviously, you will need [node.js](http://nodejs.org/).

Installation is easier with the node package manager [npm](https://npmjs.org/) :

```
npm install pd-fileutils
```

To install the command-line tool globally, you might want to run `npm` with the `-g` option. Note that in this case you might need admin rights :

```
npm install -g pd-fileutils
```


Command-line tool
------------------

At the moment, the only thing you can do is render `.pd` files to `.svg`, for example : 

```
pd-fileutils myPatch.pd > myPatch.svg
```


API documentation
-------------------

###Patch objects

`pd-fileutils` deals with JavaScript objects representing patches.

####Specification

Patch object :

```
{
    nodes: [<node1>, ..., <nodeN>],
    connections: [<connection1>, ..., <connectionN>],
    args: [<arg1>, ..., <argN>],
    layout: {<key>: <value>},
}
```

Where `<nodeK>` is with format :

```
{
    id: <id>,
    proto: <object type>,
    args:  [<arg1>, ..., <argN>],
    layout: {<key>: <value>},
    data: [<number1>, ..., <numberN>],
    subpatch: <a patch object>
}
```

- `layout` : a map containing all the layout properties of the object/patch.
- `args` : an array of the creation arguments of the object/patch. Those can be only strings or numbers.
- `data` : *[only for tables]* a list of numbers.
- `subpatch` : *[only for subpatches]* contains the whole subpatch's graph.


And `<connectionK>` :

```
{
    source: {
        id: <source object id>,
        port: <outlet>
    },
    sink: {
        id: <sink object id>,
        port: <inlet>
    }
}
```


###parse(pdFile)

Parses the string `pdFile` to a patch object. Example usage on `Node.js` :

```javascript
var pdfu = require('pd-fileutils')
  , fs = require('fs')
  , patchStr, patch

// Read the file
patchStr = fs.readFileSync('./simple.pd').toString()

// Parse the read file
patch = pdfu.parse(patchStr)
```

###renderPd(patch)

Renders the `patch` object to a string in the Pd file format.


###renderSvg(patch)

Renders the `patch` object to a string in SVG format


History
========

0.3
----

- parsing separates layout from model data
- SVG rendering of all controls
- basic pd rendering

0.2
----

- SVG rendering + command-line tool
- parsing supports most of the format 

0.1
----

- basic parsing

