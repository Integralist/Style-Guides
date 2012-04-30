#CSS Style Guide

This is my own personal style guide for writing and managing CSS. 

Feel free to take the bits you like and/or modify to your own style.

This isn't a 'righteous' guide. If there is anything here you don't like, then don't worry. I'm not forcing you to play along.

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
.m-btn {
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

As you can also see from the above example, I like to keep a space between the `property` and the `value`. I also prefer to keep each `declaration` on a separate line (it'll all get minified as part of a build process so no point in trying to optimise my source files).

Avoid use of the `!important` statement wherever possible. The only exception is when you're writing object-oriented CSS, you may need to set the 'state' for a layout/module which requires the use of `!important` (see **OOCSS** below for more details).

##Sass

I use the [Sass](http://sass-lang.com/) pre-processor via the command line.

I know there is a 'marmite - love/hate' relationship with pre-processors, and in some cases for good reason. I know the downsides and issues to be aware of and so I'm comfortable with using them.

For more information about Sass please [read my guide](https://github.com/Integralist/Blog-Posts/blob/master/Guide-to-using-SASS.md) which points out all the good features along with information on some of the features that are best avoided (from a performance perspective) if you're not sure what you're doing.

##Object-Oriented CSS (OOCSS)

I use OOCSS to ensure my stylesheets are as flexible and easily maintainable as possible.

OOCSS is very scalable and is a bit of a large subject to cover so I'll refer you to another page where [I elaborate on how I structure my CSS to fit with OOCSS](https://github.com/Integralist/Resume/blob/master/Object-Oriented-CSS.md)

##Comments

I like to add comments throughout the code to highlight any confusing areas (or anything that might not be obvious - such as working around a specific browser quirk).

Also, at the top of every stylesheet I include a heading which briefly explains the purpose of the style sheet, for example…

```css
/* =============================================================================
   Helpers
   These helpers are a lot like modules, in that they are re-usable layout utilities
   But modules are different in that they are re-usable design 'patterns'
   ========================================================================== */
   
/* =============================================================================
   Layout
   Divide the page into sections. Layouts hold one or more modules together.
   ========================================================================== */
   
/* =============================================================================
   Modules
   These are the reusable, modular parts of our design.
   ========================================================================== */
   
/* =============================================================================
   State
   The ways to describe how our modules or layouts will look when in a particular state. 
   Is it hidden or expanded? Is it active or inactive? 
   They are about describing how a module or layout looks on screens that are smaller or bigger. 
   They are also about describing how a module might look in different views like the home page or the inside page.
   ========================================================================== */
   
/* =============================================================================
   Theme
   These are similar to state rules in that they describe how modules or layouts might look. 
   Most sites don’t require a layer of theming but it is good to be aware of it.
   ========================================================================== */
```

##Mobile first

I like to implement a 'mobile first' approach which means I use a base stylesheet which displays the site as cleanly as possible on mobile devices. I then use Media Queries to load additional CSS at specific 'break points' that work well for the design.

I suggest your media queries don't target current mobile/tablet screen dimensions as in the future there may be other devices released with different dimensions. If your designs are fluid and are set to look good at specific break-points then your site should look good and be usable on pretty much any device.

##Resets

I don't like CSS resets. 

You end up writing more code to work around the lack of styling than you would have written to work around one or two browser 'quirks'.

Although I don't like CSS resets, I do like (and use) Nicolas Gallagher's [normalise.css](https://github.com/necolas/normalize.css) - *which I've modified with a few adjustments that better suit my style*. The reason I use 'normalise.css' is because it doesn't take a 'scorched earth' approach (where it wipes out all browser styles), it simply tries to normalise certain native styles (as well as try to fix some known quirky browser bugs) 

##Browser support

I write my CSS to work with [Mozilla Firefox](www.mozilla.org/en-US/firefox/).

If WebKit browsers (such as Apple Safari or Google Chrome) need any additional tweaks (which is rare but does happen), then I use a webkit hack to target those browsers… 

```css
@media screen and (-webkit-min-device-pixel-ratio:0) {
    /* WebKit rules */
}
```

After that, we only really worry about Internet Explorer (*see following section*).

##Internet Explorer

I'm still trying to find the right balance between maintainability and bloat.

At the moment I'm using IE's `conditional comments` to load IE specific style sheets… 

```html
<! --[if IE]>
<link rel="stylesheet" href="Assets/Styles/IE.css" />
<![endif]-->
```

…this helps keep my main style sheets less bloated from useless IE work arounds. But it does mean that the 'maintainability' of style sheets suffer. For example, if I edit my main CSS file(s) I might not remember that there are corresponding IE work arounds being applied. This means my IE style sheets might end up containing more rules than they actually need.

At the moment I feel that's still better than bloating out my main style sheets with irrelevant IE rules.