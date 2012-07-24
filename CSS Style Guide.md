#CSS Style Guide

This is my own personal style guide for writing and managing CSS. 

Feel free to take the bits you like and/or modify to your own style.

This isn't a 'righteous' guide. If there is anything here you don't like, then don't worry. I'm not forcing you to play along.

**UNLESS you work with me and my team: then you have to follow these rules for the sake of consistency and maintainability**

##Terminology

```css
/* Rule (the entire chunk of code below can just be referred to as a single 'rule') */

/* Selectors */
#myID, .myClass 

/* Declaration Block */
{
	/* Declaration */
	/* Property */border: /* Value */1px solid #DCDDDE;
	
	/* Declaration */
	/* Property */width: /* Value */335px;
}
```

##Style

```css
.btn {
    /* ensure prefixes line up */
	-webkit-border-radius: 20px;
       -moz-border-radius: 20px;
         -o-border-radius: 20px;
            border-radius: 20px;
	
	/* keep properties in alphabetical order (wherever possible) */
	border: 1px solid #666; /* use short-hand (wherever possible) */
	margin: 5px;
	padding: 5px;
}
```

As you can also see from the above example, you should keep a space between the `property` and the `value`. You should also keep each `declaration` on a separate line (it'll all get minified as part of a build process so no point in trying to optimise my source files).

Avoid use of the `!important` statement wherever possible. The only exception is when you're writing object-oriented CSS, you may need to set the 'state' for a layout/module which requires the use of `!important` (see **OOCSS** below for more details).

##Sass

Use the [Sass](http://sass-lang.com/) pre-processor via the command line.

I know there is a 'marmite - love/hate' relationship with pre-processors, and in some cases for good reason. As long as you know the downsides/concerns with using it then you can make sure you don't fall into any performance traps.

There are a couple of rules to try and abide to when using Sass:

* Use Sass to make you more productive - don't use all its features (functions/loops/mixins etc) just for the sake of it
* Avoid `@include` and `@extend` as both can _potentially_ be a cause of bloated CSS - aim for more abstracted code (OOCSS)
* Use `@import` in your top level `.scss` file only (where possible) - nested `@import` statements can get confusing otherwise
* Don't nest selectors as this can result in badly performing selectors - make sure you understand the CSS that nested selectors generates
* Use braces and proper CSS formatting (some pre-processors allow you to ignore braces - this makes code hard to read for others not familiar with the syntax)

For more information about Sass please [read my guide](https://github.com/Integralist/Blog-Posts/blob/master/Guide-to-using-SASS.md) which discusses how to install and use Sass.

##Object-Oriented CSS (OOCSS)

Use OOCSS to ensure stylesheets are as flexible and easily maintainable as possible.

OOCSS is very scalable/flexible but is a bit of a large subject to cover, so for more information I'll refer you to my other post where [I elaborate on how to structure your CSS to fit the OOCSS methodology](https://github.com/Integralist/Resume/blob/master/Object-Oriented-CSS.md)

##Comments

Add comments throughout the code to highlight any confusing areas (or anything that might not be obvious - such as working around a specific browser quirk).

Also, at the top of every stylesheet include a heading which briefly explains the purpose of the style sheet, for example…

```css
/* =============================================================================
   Theme
   These are base level rules, but which are specific to the current project
   ========================================================================== */
```

##Mobile first

If you have the budget/time then aim to Implement a 'mobile first' approach which means you'll use a base stylesheet which displays the site as cleanly as possible on mobile devices. Then use Media Queries to load additional CSS at specific 'break points' that work well for the design.

Your media queries should (ideally) not target current mobile/tablet screen dimensions - because in the future there may be other devices released with different dimensions that you haven't catered for. If your designs are fluid and are set to look good at specific break-points then your site should look good and be usable on pretty much any device.

##Resets

Don't use CSS resets. 

You end up writing more code to work around the lack of styling than you would have written to work around one or two browser 'quirks'.

Do use Nicolas Gallagher's [normalise.css](https://github.com/necolas/normalize.css) - *and feel free to add any modifications required so it fits the company's house style*. The reason you should use 'normalise.css' is because it doesn't take a 'scorched earth' approach (i.e. wipes out all browser styles), it simply tries to normalise certain native styles (as well as try to fix some known quirky browser bugs) 

##Browser support

Write your CSS to work first with [Mozilla Firefox](www.mozilla.org/en-US/firefox/) as it is the most unbiased and standard compliant browser available.

Once your CSS is working on Firefox then you can look at any issues with other browsers. This doesn't mean *not check* other browsers while developing. You should be regularly checking your code against all supported browsers during development - this way you catch out any quirks early.

If WebKit browsers (such as Apple Safari or Google Chrome) need any additional tweaks (which is rare but does happen), then use a webkit hack to target those browsers… 

```css
@media screen and (-webkit-min-device-pixel-ratio:0) {
    /* WebKit rules */
}
```

After that, you only really need to worry about Internet Explorer (*see following section*).

##Internet Explorer

To work around some of the quirks in IE's rendering you should use Microsoft's Conditional Comments, but NOT to load in additional style sheets just for IE like the following example...

```html
<!--[if IE 8]>
<link rel="stylesheet" href="/Assets/Styles/IE8.css">
<![endif]-->

<!--[if IE 7]>
<link rel="stylesheet" href="/Assets/Styles/IE7.css">
<![endif]-->
```

...instead use [Paul Irish's solution](http://paulirish.com/2008/conditional-stylesheets-vs-css-hacks-answer-neither/)…

```html
<!--[if IE 8]><html class="ie8" dir="ltr" lang="en"><![endif]-->
<!--[if IE 9]><html class="ie9" dir="ltr" lang="en"><![endif]-->
<!--[if gt IE 9]><!--> <html dir="ltr" lang="en"> <!--<![endif]-->
```

...this helps keep your CSS modules together and more easily maintainable.

Since we dropped IE7 support AND implemented a more modular (OOCSS) approach to writing our CSS we've found IE related bugs (although still apparent) have pretty much fallen off the radar compared to previously!