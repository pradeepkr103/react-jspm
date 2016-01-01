
# React with JSPM.io an Introduction

## Intro - Learning React with a JSPM toolchain

This is a series of examples for React.js using a JSPM.io and ES6 toolchain.

You should have done the React Raw Intro sequence before this and the ES6 intro.

This repo has examples of increasing difficulty.


* [Ex 21_jspmcart](Ex 21_jspmcart)


## Webpack

You can also use the react-webpack-redux toolchain and repo (future todo).



# JSPM.io Intro - Theory

## Concepts

1. packaging and modules system (via SystemJS) 
	* 
	- setup package.json, , config.js, babel as transpiler
	- jspm_packages 
		- stores dependencies from MULTIPLE sources git, npm, bower 


2. Dependencies - config.js setup
	*
	- all dependences treated equally 
	- JSPM plugins get injected in dependency order
	-  you are able to rename your packages to the name you like the most.
		$ jspm install npm:jspm-loader-jsx  #config.js would have jspm-loader-jsx named as default name
		$ jspm uninstall jspm-loader-jsx    #config.js removes this
		$ jspm install jsx=npm:jspm-loader-jsx  #config.js would have jspm-loader-jsx named as jsx
		# config.js changes .. 
		map: {  // "jspm-loader-jsx": "npm:jspm-loader-jsx@0.0.7"
		    "jsx": "npm:jspm-loader-jsx@0.0.7"
		}

* Versioning/Mgt
	> jspm CLI implements shortest-path fork reduction during installs, while ensuring targets are at their latest patch versions to reduce bugs. The version solution is created within the project configuration file, and can be checked-in to version control to lock dependencies.


2b. Precompile JSX to JS
	- In JS code just do
		import jsx   // but don't need as jspm-loader-jsx as JSPM plugin gets injected in dependency order

3. Strong ES6 Support
	* Add following to customize babel .. support all the experimental features like class properties.
	* This seems to be default .. except for stage : 0
		babelOptions: {
		    "stage": 0,
		    "optional": [
		      "runtime",
		      "optimisation.modules.system"
		    ]
		  }

4. Dynamic Module Loading top down
	- SystemJS looks at config.js and allows import of top module and its own dependenciess
	<script src="jspm_packages/system.js"></script>
	<script src="config.js"></script>
	<script> System.import('app.jsx!'); </script>

	NOTE: the ! sign in app.jsx!. This is a convention used in JSPM for loading non-JavaScript files.


5. MULTIPLE File and Module formats supported - require.js also
	https://github.com/systemjs/systemjs/blob/master/docs/module-formats.md

6. Multiple files supported all through import only
* In HTML use script system.js
	<script> System.import('app.jsx!'); </script> 
* In JS use import xxxx
	- .js is default import
	- .jsx!
	- .css!     import "./pathto/style.css!"


7. Building for production - concatenate & minify
	- JSPM understands css, js, etc and handles them specially 
	jspm bundle-sfx app.jsx! app.js --skip-source-maps --minify

	For JSPM production workflow SEE: https://github.com/jspm/jspm-cli/blob/master/docs/production-workflows.md


## Official and Resources

* JSPM.IO

* https://github.com/jspm/jspm-cli/wiki

* https://github.com/jspm


## JSPM Packages, Modules Management

* Tutes Package Management for ES6 Modules JSConf 2014 presentation introducing jspm by Guy Bedford
https://www.youtube.com/watch?v=szJjsduHBQQ

SystemJS: https://github.com/systemjs/systemjs
	Module Loaders - https://github.com/systemjs/systemjs/blob/master/docs/module-formats.md

* REGISTRY - Common JSPM names packages registry simplifies install 
	- https://github.com/jspm/registry
	https://github.com/jspm/jspm-cli/blob/master/docs/installing-packages.md

	$jspm install jquery # vs jspm install github:components/jquery

JSPM plugins https://github.com/jspm/jspm-cli/blob/master/docs/plugins.md

JSPM production workflow https://github.com/jspm/jspm-cli/blob/master/docs/production-workflows.md


## Tutorials

Getting Started official JSPM http://jspm.io/docs/getting-started.html

JavaScript Modules and Dependencies with jspm tutorial by Jack Franklin 
http://javascriptplayground.com/blog/2014/11/js-modules-jspm-systemjs/

d http://egorsmirnov.me/2015/10/11/react-and-es6-part5.html

http://www.joezimjs.com/javascript/simplifying-the-es6-workflow-with-jspm/

http://blog.developsuperpowers.com/jspm-superpowered-es6-development/

https://gitter.im/jspm/jspm

