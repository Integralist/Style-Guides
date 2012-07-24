#JavaScript Style Guide

This is my own personal style guide for writing JavaScript. 

Feel free to take the bits you like and/or modify to your own style.

This isn't a 'righteous' guide. If there is anything here you don't like, then don't worry. I'm not forcing you to play along.

**UNLESS you work with me and my team: then you have to follow these rules for the sake of consistency and maintainability**

One thing to be aware of is that throughout this guide, to illustrate our style over another, we will mark something as 'bad' and then mark what we prefer as 'good'. We'm fully aware that this is not the best choice of phrase because one way isn't necessarily 'bad', it's just not our preferred way. So please don't hate on us for doing this, it's just the simplest way to demonstrate my preferred format. 

Here is what we'll cover:

* Terminology
* Style
	* Basics
	* Code structure
	* Loops
	* Ternary operator
	* Spacing
	* Comparison
	* Coercion
	* Naming Conventions
	* Returning a value
* DOM (Document Object Model)
* Templating
* Modular
* Application structure
* Linting
* Build process
* Performance considerations
* Unit testing
	* Test-Driven Development
	* Testing frameworks
* jQuery


##Terminology

For seasoned developers this section isn't necessary, but sometimes it is useful to clarify some of the following terms to ensure all developers on the team understand what they refer to… 

Term                  | Example
--------------------- | -------------
'expression'          | An expression is something a JavaScript engine can interpret as a value
'statement'           | A statement is code that can be executed (statements are terminated with a semicolon)
'function declaration'| `function myFunction(){ /* code */ }`
'function expression' | `var myFunction = function(){ /* code */ };`

##Style

###Basics

* Always use semicolons (controversial nowadays) - do not rely on the JavaScript engine to handle ASI ('*Automatic Semicolon Insertion*').
* Use single quotes `''` instead of double quotes `""` as they look cleaner/clearer when scaning code (the exception to this rule is when you're adding a string of text, as you're more likely to need to escape a `"` than `'` -  
e.g `"I'm stuck here until they're finished working"`).
* Never mix spaces and tabs.
* Use four spaces to represent a single tab.
* Use multiple var statements (not one var statement for multiple variables - let your minifier/build script handle that for you)
	* With the exception for variables have no value: `var name, age, location;` and should come last in the list of variables.

###File structure

* __Code Structure__  
Appears once at the top of every .js file
* __Function Information__  
Appears at the top of each function and is specific to that function
* __Variables__  
Define variables as close to the top of their scope as possible
* __Supporting functions__  
Inner functions should always follow the variable declarations and be in as logical 'order' as possible
* __Code__  
This is where the main crux of the function's code will sit

```js
/*
 * Code Structure:
 * - Variables
 * - Functions
 *   - fn_name_1
 *   - fn_name_2
 *   - fn_name_3
 *   - fn_name_4
 *   - fn_name_5
 *   - fn_name_6
 *   - fn_name_7
 *   - fn_name_8
 *   - fn_name_9
 * - Initialisation
 */

/*
 * This is the "Function Information".
 * It should be short and to the point.
 * 
 * @param abc {String} describe what this argument is
 * @param xyz {Integer} describe what this argument is
 */
function myFunction (abc, xyz) {
	var name = 'Mark';
	var location = 'England';
		
	function doSomething (withThis) {
		// do something
	}
	
	// main code for this function
}
```

###Loops

Don't use `for` loops unless you feel you absolutely must. There is a micro-optimisation that can be had from using a `while` loop instead, but generally `for` loops are harder to read and understand compared to `while` loops… 

```js
// Bad
var arr = ['a', 'b', 'c'];
for (var i = 0, len = arr.length; i < len; i++) {
	// do something
}

// Good
var arr = ['a', 'b', 'c'];
var len = arr.length;
	
while (len--) {
	// do something 
	// note: this loop will run in reverse - this is the fastest way to loop
	// most of the time the direction of your loop isn't important.
}
```

…but if you're worried about your `while` loop going backwards, you can still go forwards and have it look cleaner than the `for` loop (IMO)…

```js
var arr = ['a', 'b', 'c'];
var len = arr.length;
var counter = 0;
	
while (counter < len) {
	// do something
	
	counter++;
}
```

###Ternary operator

Only use the ternary operator `?:` for short if/else statements.

```js
// Bad
if (condition) {
	// do something when condition is true
} else {
	// do something when condition is false
}

// Good
(condition) ? /* condition was true */ : /* condition was false */;
```

###Spacing

Our style of spacing is a little more complicated than others because it changes depending on the context… 

```js
// No arguments: no space around parenthesis

function myFunction(){
	// code
}
var myFunction = function(){
	// code
};

// Arguments: keep a single space around parenthesis
function myFunction (a, b, c) {
	// code
}
var myFunction = function (a, b, c) {
	// code
};

// Loops and Conditionals
if (condition) {
	// code
}
while (condition) {
	// code
}
for (condition) {
	// code
}
```

…also, if a function is quite long then it can be clearer to have a space inside of the brackets… 

```js
// This is a function taken from Underscore.js
function bind (func, context) {
	
	var bound, args;
	
    if (func.bind === nativeBind && nativeBind) {
    	return nativeBind.apply(func, slice.call(arguments, 1));
    }
    
    if (!_.isFunction(func)) {
    	throw new TypeError;
    }
    
    args = slice.call(arguments, 2);
    
    return bound = function(){
    
		if (!(this instanceof bound)) {
			return func.apply(context, args.concat(slice.call(arguments)));
		}
		
		ctor.prototype = func.prototype;
		
		var self = new ctor;
		var result = func.apply(self, args.concat(slice.call(arguments)));
		
		if (Object(result) === result) {
			return result;
		}
      
      return self;
      
    };
	
}
```

###Comparison

Use strict equality `===` over `==`.

The only exception is when you're checking the result from the `typeof` operator, as it always returns a string so there is no point using `===`.

###Coercion

Take advantage of JavaScript's ability to coerce objects into different types.

```js
var element = document.getElementById('js-element');

if (element) {
	// if the DOM element is available then this conditional
	// will coerce `element` into a Boolean
}
```

You can coerce a value into a Boolean by using the double negation operator `!!`...

```js
var obj = { age: 0, year: 1980 };

!!obj.age // => false (because zero coerces to false)
!!obj.year // => true (any number greater than zero coerces to true)
```

All Objects/Arrays coerce to true (even an empty Array).

An empty String wil coerce to false.

For full details see [http://webreflection.blogspot.co.uk/2010/10/javascript-coercion-demystified.html](http://webreflection.blogspot.co.uk/2010/10/javascript-coercion-demystified.html)

###Naming Conventions

Define long names using an underscore as it is easier to read… 

```js
var user_location = 'England';

function get_user_location(){
	// code
}
```

###Returning a value

Try to `return` a function as early as possible, as it makes it clearer when a function fails/succeeds.

###Asynchronous code

Whenever you're working with asynchronous code, consider the use of [Promises](http://wiki.commonjs.org/wiki/Promises) to make your code cleaner and easier to manage…  

> Promises provide a well-defined interface for interacting with an object that represents the result of an action that is performed asynchronously, and may or may not be finished at any given point in time. By utilizing a standard interface, different components can return promises for asynchronous actions and consumers can utilize the promises in a predictable manner. Promises can also provide the fundamental entity to be leveraged for more syntactically convenient language-level extensions that assist with asynchronicity. 

Our library of choice is [When.js](https://github.com/cujojs/when). It makes it a lot easy to manage asynchronous operations:

```js
define(['when', 'swfobject', 'async!http://gdata.youtube.com/feeds/api/videos?author=xxxx&alt=json'], function (when, swf, videos) {

	var global = (function(){return this;}());
	var doc = document;
	var id = videos.feed.entry[0].id.$t.split('videos/')[1];
	var flash = doc.createElement('div');
	var container = doc.getElementsByTagName('div')[2];
	var params, atts;

	function async (template) {
		var dfd = when.defer();
		var tmp, timer;
		
		template({ 
			title: 'Flash content inserted via JavaScript using a template to render content'
		}),
		
		// Because template() function is asynchronous (and no callback built-in)
		// we use a timer to keep track of 'tmp' value
		timer = global.setInterval(function(){ 
			(!!tmp) 
				? (global.clearInterval(timer), dfd.resolve(tmp)) 
				: null; 
		}, 25);		
		
		return dfd.promise;
	}
		
	function handler(){
        require(['tpl!../Templates/Video.tpl'], function (template) {
			
			when(async(template), function (htmlFragment) {
				var frag = doc.createDocumentFragment();
				var div = doc.createElement('div');
				
				div.innerHTML = htmlFragment;
				frag.appendChild(div);
				container.insertBefore(frag, doc.getElementById('currentvideo'));
            });
            
        });
	}
	
	flash.id = 'insertflash';
	atts = { id: 'currentvideo' };
	
	container.appendChild(flash);
	
	swf.embedSWF('http://www.youtube.com/v/' + id + '?enablejsapi=1&playerapiid=ytplayer&version=3', 'insertflash', '270', '150', '8', null, null, null, atts, handler);
	
});
```

...but note Promises/Deferred objects are available with jQuery.

##DOM (Document Object Model)

Don't touch the DOM unless you absolutely must, as it can be a real performance overhead. Make sure you cache DOM elements and properties/values and re-use elements wherever possible.

For example, if you create an element `var div = document.createElement('div')` and you find you need another `div` element then just re-use that previous variable `var new_div = div.cloneNode();` and if you're referencing a property such as `document` more than twice then make sure you store it in a variable… 

```js
var doc = document;
var element_a = doc.getElementById('testA');
var element_b = doc.getElementById('testB');
var divs = doc.getElementsByTagName('div');
```

When adding style settings to an element use a `class` instead… 

```js
var element = doc.getElementById('test');
	
// Bad
element.style.border = '1px solid red';
element.style.backgroundColor = 'yellow';

// Good
element.className = 'style_a';
```

…and in a separate CSS you would have… 

```css
.style_a {
	border: 1px solid red;
	background-color: yellow;
}
```

##Templating

JavaScript templating is related to the DOM in that it allows you to insert dynamic data into HTML much more easily and cleanly.

Normally you would use JavaScript to locate HTML in your page and then update and display the content (note: you could also be creating HTML using `innerHTML` or via the DOM API). But this means that you now have HTML being written inside your JavaScript which goes against the tenet of 'separating your concerns'. 

Instead you should have any modules(HTML) within your page/web application that will be dynamically updated placed inside separate template files which you load into memory using AJAX and then process the templates by passing in the relevant data and once rendered you can insert the template into the DOM.

There are many templating libraries available but we use [Hogan.js](http://twitter.github.com/hogan.js/) which is modelled from [Mustache](https://github.com/janl/mustache.js#readme).

Here follows is an example of a template and how to render it using Hogan… 

```html
<div>
	<p>{{title}}</p>
</div>
```

```js
ajax({
	url: 'Assets/Templates/Video.tpl',
	data: 'html',
	onSuccess: function (data) {
		var template = hogan.compile(data);
		var content = template.render({ 
				title: 'My Title' 
			});
		var frag = doc.createDocumentFragment();
		var div = doc.createElement('div');
		
		div.innerHTML = content;
		frag.appendChild(div);
		container.insertBefore(frag, target);
	}
});
```

##Modular

Write your JavaScript to be AMD compatible. This means having multiple scripts written like 'modules' which you call into your main script when needed… 

```js
require(['module_a', 'module_b', 'module_c'], function (a, b, c) {
	// do something with the returned values from each module
});
```

…and a module is written like so… 

```js
define(['module_d'], function (d) {

	// do something with module 'd' 
	// optionally return a value/object/function etc

});
```

If you don't like using AMD then you can get away with using an anonymous function to protect the global environment from stray variables and other settings… 

```js
(function (global) {
	
	// code
	
}(this));
```

…but there is no real organisation/structure of code this way.

**NOTE: if  you work with me and my team: then you have to follow the AMD rule for the sake of consistency and maintainability**

##Application structure

Having a clear and easy to undestand directory architecture is important. Don't just have all your JavaScript files dumped inside a single folder. Instead have a defined structure for your applications.

The following is an example: 

* Assets 
	* Styles
	* Scripts
		* Application
		* Build
		* Lint
		* Plugins
		* Tests
		* Utilities

…this gives you a clear separation of modules into an organised structure.

##Linting

Use [JSHint](http://www.jshint.com/) via the command line interface to find syntax errors in your code.

This helps keep a consistency of style and prevents issues when code is minified via a build script.

Some things JsHint ensures:

* Always put curly braces around blocks in loops and conditionals
* Prohibits the use of `==` and `!=` in favor of `===` and `!==` (any time you're checking a `typeof` result you should use `==` and just ignore any warnings about using `===` because `typeof` ALWAYS returns a String so no need for the strict equality check)

One error you might see is the one asking you to use `hasOwnProperty` when checking properties inside of a `for in` loop. 99% of the time you'll know it wont be an issue, but if you start extending natives (such as `Array`) then you open yourself up to the possibility of un-intended results that cause your code to error.

###Configuration

The configuration file for JSHint is as follows… 

```js
{
	// Settings
    'passfail'      : false,  // Stop on first error.
    'maxerr'        : 200,    // Maximum error before stopping.


    // Predefined globals whom JSHint will ignore.
    'browser'       : true,   // Standard browser globals e.g. `window`, `document`.

    'node'          : false,
    'rhino'         : false,
    'couch'         : false,
    'wsh'           : false,   // Windows Scripting Host.

    'jquery'        : true,
    'prototypejs'   : false,
    'mootools'      : false,
    'dojo'          : false,

    'predef'        : [  // Custom globals.
    	// this is because we use require() from RequireJS library
        'require',
        'define',
        
        // this is because we use Jasmine BDD for unit-testing
        'jasmine',
        'describe',
        'beforeEach',
        'afterEach',
        'it',
        'expect'
    ],


    // Development.
    'debug'         : false,  // Allow debugger statements e.g. browser breakpoints.
    'devel'         : false,  // Allow developments statements e.g. `console.log();`.


    // ECMAScript 5.
    'es5'           : true,   // Allow ECMAScript 5 syntax.
    'strict'        : false,  // Require `use strict` pragma  in every file.
    'globalstrict'  : false,  // Allow global 'use strict' (also enables 'strict').


    // The Good Parts.
    'asi'           : false,  // Tolerate Automatic Semicolon Insertion (no semicolons).
    'laxbreak'      : true,   // Tolerate unsafe line breaks e.g. `return [\n] x` without semicolons.
    'bitwise'       : true,   // Prohibit bitwise operators (&, |, ^, etc.).
    'boss'          : true,   // Tolerate assignments inside if, for & while. Usually conditions & loops are for comparison, not assignments.
    'curly'         : true,   // Require {} for every new block or scope.
    'eqeqeq'        : true,   // Require triple equals i.e. `===`.
    'eqnull'        : false,  // Tolerate use of `== null`.
    'evil'          : false,  // Tolerate use of `eval`.
    'expr'          : false,  // Tolerate `ExpressionStatement` as Programs.
    'forin'         : false,  // Tolerate `for in` loops without `hasOwnPrototype`.
    'immed'         : true,   // Require immediate invocations to be wrapped in parens e.g. `( function(){}() );`
    'latedef'       : true,   // Prohipit variable use before definition.
    'loopfunc'      : false,  // Allow functions to be defined within loops.
    'noarg'         : true,   // Prohibit use of `arguments.caller` and `arguments.callee`.
    'regexp'        : false,  // Prohibit `.` and `[^...]` in regular expressions.
    'regexdash'     : false,  // Tolerate unescaped last dash i.e. `[-...]`.
    'scripturl'     : true,   // Tolerate script-targeted URLs.
    'shadow'        : true,   // Allows re-define variables later in code e.g. `var x=1; x=2;`.
    'supernew'      : false,  // Tolerate `new function () { ... };` and `new Object;`.
    'undef'         : true,   // Require all non-global variables be declared before they are used.


    // Personal styling preferences.
    'newcap'        : true,   // Require capitalization of all constructor functions e.g. `new F()`.
    'noempty'       : true,   // Prohibit use of empty blocks.
    'nonew'         : true,   // Prohibit use of constructors for side-effects.
    'nomen'         : true,   // Prohibit use of initial or trailing underbars in names.
    'onevar'        : false,  // Allow only one `var` statement per function.
    'plusplus'      : false,  // Prohibit use of `++` & `--`.
    'sub'           : false,  // Tolerate all forms of subscript notation besides dot notation e.g. `dict['key']` instead of `dict.key`.
    'trailing'      : false,  // Prohibit trailing whitespaces.
    'white'         : true,   // Check against strict whitespace and indentation rules.
    'indent'        : 4,      // Specify indentation spacing
    'smarttabs'		: true	  // Suppress warnings about mixed tabs and spaces
}
```

##Build process

A build tool/script/process is a way of making your code/project as efficient and performant as possible. Build scripts can do things like… 

* Combine and minify JavaScript and CSS
* Optimise images (e.g. stripping out unnecessary meta data and shrinking overall file size)
* Strip out any development code (e.g. console.log)
* Lint your code

…and loads of other things depending on the tool you use.

###JavaScript Build Script

For our JavaScript, because we use AMD and specifically [RequireJs](http://requirejs.org/), we also use RequireJs' own build script which is called 'r.js'.

The way 'r.js' works is that you execute a configuration file via the command line (using either Java or Node.js)

The 'r.js' build script takes all our JavaScript 'modules' and combines them into a single file that only contains the relevant modules required for that section of our application to run. 

There are many build tools available, some of which are:

* Grunt (which is a task-based command line build tool for JavaScript projects)
* ANT (which is a Java based build tool and which has lots of build scripts designed to work with it; such as the [HTML5 Boilerplate](https://github.com/h5bp/ant-build-script)). 

As long as you're using some form of build script your application will be better for it.

##Performance considerations

Performance is a big issue but can easily turn into micro-optimisations.

For example, some things you can do (which are definitely micro-opts):

* Use `~~` over `Math.floor`
* Use reverse `while` loop instead of forward `for` loop (*BUT has the bonus of being clearer to read/understand*)

The best approach is to try and not to make things too complicated. But if you want to use some bizarre micro-optimisation technique then make sure you comment it well and include links to articles about the technique so that other developers who look at your code don't get confused as to what your cryptic piece of code means.

One performance trick which is good to do is caching (try to cache whenever you can). Caching can be handled in a variety of different ways depending on what you want to cache. You could cache a heavy computation, or a variable/property to aid scope lookups, or you could cache a DOM object. 

One way to cache results of a heavy computation is through memoization. One thing to remember is that the memoization technique doesn't have to be used for just 'heavy computations', it can be used in any instance where you want to save the JavaScript engine from having to do extra work unnecessarily. The following example demonstrates this… 

```js
/**
 * The following method creates a new element or returns a copy of an element already created by this script.
 *
 * @param tagname { String } element to be created/copied
 * @return { Node } the newly created element node
 */
var createElement = (function(){
	// Memorize previous elements created
	var cache = {};
	
	return function(tagname) {
		if (!(tagname in cache)) {
			// Create new instance of specified element and store it
			cache[tagname] = document.createElement(tagname);
		}
		
		// If we've already created an element of the specified kind then duplicate it
		return cache[tagname].cloneNode(false);
	}
}());
```

For testing performance use the online tool [jsPerf](http://jsperf.com/). It lets you write different test cases for a particular piece of code. For example if you wanted to see if a `while` loop was faster than a `for` loop then you would write a test that executed both loops against a familiar piece of code and jsPerf would execute that code repeatedly to see which variation performed better (e.g. [while vs for](http://jsperf.com/yet-another-for-vs-while/2))

##Unit testing

Testing your code is very important and we should all do it more. 

But we appreciate why this doesn't happen as often as it should.

###Test-Driven Development (TDD)

Unit testing and Test-Driven Development are two different things. Do not get them confused.

If you write code and then decide to write tests for that code: **you are doing 'unit testing'**. 

If you write tests (without any code to test against) and then you write enough code to make the tests pass and you 'rinse/repeat' that process until you have your code base complete: **you are doing 'test-driven development'**.

###Testing frameworks

There are literally 'tons' of different testing frameworks available, some are 'test-driven', others are 'behaviour-driven', others are command line, others let you test against multiple browsers at once - the list goes on.

Some of interest… 

* [Buster.Js](http://busterjs.org/) (and my personal favourite)
* [Jasmine](http://pivotal.github.com/jasmine/) (see an article I've written about [using Jasmine](https://github.com/Integralist/Blog-Posts/blob/master/Beginners-guide-on-how-to-test-your-code-%28using-Jasmine-BDD%29.md) which also covers information about unit testing, the 'write first' process and TDD vs BDD).
* [jsTestDriver](https://code.google.com/p/js-test-driver/)
* [JsUnit](http://www.jsunit.net/)
* [QUnit](http://docs.jquery.com/QUnit)
* [Sinon](http://sinonjs.org/) (this is an add-on to testing frameworks for Spies/Stubs/Mocks)

Another testing tool is called [Testling](http://testling.com/) which is a command line tool and lets you test your code against a suite of browsers without needing to have them installed (it tests them remotely). Testling can be used as follows… 

```js
/*
Create account with http://browserling.com/
Run tests via http://testling.com/

Documentation: http://testling.com/docs/

Execute test (via command line): 
	curl -u me@example.com:password -sSNT mytest.js testling.com/?browsers=iexplore/6.0,iexplore/7.0,safari/5.0
	curl -u me@example.com:password -sSNT mytest.js testling.com/?browsers=iexplore/6.0,firefox,safari

With a free account you get 200 minutes per month.
To check your usage (via command line):
	curl -u me@example.com -s testling.com/usage
*/
var test = require('testling');

test('the test name', function (t) {
    // See all test assertions here: 
    // http://testling.com/docs/#test-assertions
    
    t.equal(2 + 2, 4);
    
    t.ok(window.JSON, 'JSON is natively supported');
    
    t.log(window.navigator.appName);

    t.end();
});
```

##jQuery

If you're using jQuery then there are only a few rules to abide by...

###Cache jQuery instances at all possible points 
 
```
var test = $('#js-element');
test.addClass('is-active');
test.removeClass('is-hidden');

// DO NOT DO...
$('#js-element').addClass('is-active');
$('#js-element').removeClass('is-hidden');

// ...AS THIS MEANS YOU'RE CREATING TWO NEW jQuery INSTANCES. 
// PLUS YOU'RE UNNECESSARILY LOOKING UP THE SAME ELEMENT TWICE
```

###Avoid jQuery's `.css()` method as that creates an 'inline style' which can be an expensive DOM interaction - instead aim to use `.addClass()`  

```
var test = $('#js-element');

// SO INSTEAD OF THIS...
test.css({backgroundColor: '#ffe', borderLeft: '5px solid #ccc'});

// ...DO THIS...
test.addClass('is-highlighted'); // .is-highlighted is set via your CSS
```

###Use the latest version release of jQuery

###Use the latest API 
e.g. avoid `.click()` and use `.on()` instead


###Use event delegation where ever possible 
Do not have multiple individual `click` events for one element - e.g. click event for every `<li>` in a un-ordered list

###Don't use jQuery's abstracted AJAX methods `post()` and `get()`
Just use the `ajax()` method as it's clearer/more specific and allows error handler to be used.

###If you must use jQuery to search for elements then the quickest way to do that is with the `.find()` method  

```
var test = $('#js-element'); // this is a <div> that contains a <ul> list
var list_items = test.find('li');

// OR

var test = $('#js-element'); // this is a <form> that contains a list of input options
var list_items = test.find('.selected');
```

###In general...
Avoid jQuery unless absolutely necessary.