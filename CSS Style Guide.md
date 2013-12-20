<<<<<<< HEAD
# CSS Style Guide
=======
#CSS Style Guide

This is my own personal style guide for writing and managing CSS. 

Feel free to take the bits you like and/or modify to your own style.

Here is what we'll cover:
>>>>>>> 4b3390062afb4d83c953522108997311a781656a

- Summary
- Terminology
- Style
- Sass
- Object-Oriented CSS (OOCSS) via BEM
- Padding, margin, borders
- Abstractions?
- Comments
- Mobile first
- Resets
- Browser support
- Sprites
- Responsive Design
- Responsive Images
- What about containing elements with unknown widths?
- JavaScript Components

##Summary

1. Use the BEM (Block Element Modifier) methodology 
2. Find common patterns in your code and abstract them so they can become reusable components. 
3. Think about how you can write more clean, efficient and modular components
4. Be critical of your code and the code of others. Try and learn/recognise 'code smells' and refactor them. 

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

- Use 4 spaces for indentation
- Avoid using IDs, as classes are more reusable
- Write efficient and targeted selectors (the more generic your selectors, the higher the chance HTML changes will cause your code to break)
- Avoid over qualifying selectors (e.g. `p.qualified {…}` should just be `.qualified`)
- Avoid chaining classes (`.chained.classes`) as this shouldn't be necessary (see OOCSS section below)
- Ideally (depending on browser support) utilise the `box-sizing: border-box` model (supported on IE8+)
- Avoid the use of the `!important` statement wherever possible. The only exception to that rule is when (in the process of writing object-oriented CSS) you may need to set the 'state' for a component and depending on the situation it may require the use of `!important`. The setting of 'state' is the only time you are allowed to use `!important`
- Rely as much as possible on the patterns/abstractions available within the Guts framework as this will mean you are writing less boilerplate code.
- Use `rem` unit values for font sizing
- Use `em` unit values for padding/margin where possible.

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

As you can also see from the above example, you should keep a space between the `property` and the `value`. You should also keep each `declaration` on a separate line (it'll all get minified as part of a build process so no point in trying to optimise your source files).

##Sass

Use the [Sass](http://sass-lang.com/) pre-processor via the command line.

There are a couple of rules to try and abide to when using Sass:

* Use Sass to make you more productive - don't use all its features (functions/loops/mixins etc) just for the sake of it
* Avoid `@extend` (where possible) as it can _potentially_ be a cause of bloated CSS (unless you're extending a placeholder: `%someContent { color: red; }`) -> but in general aim for more abstracted code (OOCSS)
* Use `@import` in your top level `.scss` files only (where possible) - nested `@import` statements can get confusing otherwise
* Use single quotes when referencing strings (e.g. `@import 'guts/base'` and not `@import "guts/base"`)
* When using a mixin, make sure that you group all mixins at the top of the rule like so...
    ```scss
    .my-rule {
        @include a();
        @include b();
        @include c();
        @include d();
        line-height: 1;
        letter-spacing: $letter-spacing;
    }
    ```
* Don't nest selectors as this can result in badly performing selectors - if you need to nest then make sure you understand the CSS that nested selectors generates
* If you _must_ nest selectors/rules then do not go further than 3 levels deep.
* Use braces and proper CSS formatting (some pre-processors allow you to ignore braces - this makes code hard to read for others not familiar with the syntax)
* Use the `&` character to reference the parent selector (this is also very useful for IE fixes - where you use the `&` character at the end of your selector to have wrapped by the specific parent selector. If that sounds confusing then [here is an example](https://gist.github.com/3226822).
* __DO NOT__ do this (it's pointless and doesn't add any benefit)...  
    ```scss
    %something-placeholder {
        @include a();
        @include b();
        @include c();
        @include d();
        line-height: 1;
        letter-spacing: $letter-spacing;
    }
    
    @mixin something {
        @extend %something-placeholder;
    }
    ```

### Order of a rule's content...

List `@extend`(s) First:

```scss
.weather {
  @extends %module; 
  ...
}
```

List `@include`(s) Next

```scss
.weather {
  @extends %module; 
  @include vendor(transition, all 0.3s ease-out);
  ...
}
```

List "Regular" Styles Next

```scss
.weather {
  @extends %module; 
  @include vendor(transition, all 0.3s ease-out);
  color: red;
}
```

Nested Selectors Last

```scss
.weather {
  @extends %module; 
  @include vendor(transition, all 0.3s ease-out);
  
  color: red;

  > p {
    // not that you would use a nested selector here (you'd be using BEM to try and negate a need for nested rule)
  }
}
```

### List Vendor/Global Dependancies First, Then Author Dependancies, Then Patterns, Then Parts

```scss
/* Vendor Dependencies */
@import "compass";

/* Authored Dependencies */
@import "global/colors";
@import "global/mixins";

/* Patterns */
@import "global/tabs";
@import "global/modals";

/* Sections */
@import "global/header";
@import "global/footer";
```

The dependencies like Compass, colors, and mixins generate no compiled CSS at all, they are purely code dependancies. Listing the patterns next means that more specific "parts", which come after, have the power to override patterns without having a specificity war.

###Some basic styling examples... 

```scss
.selector {
  // Current selector's styles are set before nesting
  property: value;
  property2: value;

  p { // direct descendent not ideal as it could cause design problems when HTML changes!
    property:value; // Not good
    property: value; // Good
  }
  .className {} // Not good
  .CLASSNAME {} // Not good
  .class_name {} // Not good
  .class-name{} // Not good
  .class-name {} // Good

  // Successive selectors (one per line)
  .selector-1, .selector-2, .selector-3 {} // Not good
  .selector-1,
  .selector-2,
  .selector-3 {} // Good

  // One liners
  .one-liner { margin: 0; border-radius: 5px; font-size: 15px; } // Not good
  .one-liner { margin: 0 } // OK: one property (notice with one property we don't need a semi-colon at the end)

  // Modifiers after base styles
  &.modifier {
    p { // overridden descendant

    }
  }

  .indentation-tricks {
    // Aligned values are more readable
    $var-alpha:   15px !default;
    $var-epsylon: true;
    $var-zeta:    false;
  }

  @media (min-width: 680px) {
    // Alterations for this breakpoint
    // With Sass you can nest @media queries
  }

  // Conditions
  @if $compact {
    // Case 1
  } @else {
    // Case 2
  }
}
```

##Object-Oriented CSS (OOCSS) via BEM

Use OOCSS to ensure stylesheets are as flexible and easily maintainable as possible.

An OOCSS methodology can make your CSS much more scalable, flexible and maintainable but is a bit of a large subject to cover, so for more information I **strongly** advise you to read the following posts...

- [Maintainable CSS with BEM](http://www.integralist.co.uk/posts/maintainable-css-with-bem/)
- [Getting your head around BEM syntax](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)
- [Code smells in CSS](http://csswizardry.com/2012/11/code-smells-in-css/)
- [Using more classes in your HTML](http://csswizardry.com/2012/10/a-classless-class-on-using-more-classes-in-your-html/)
- [CSS selector intent](http://csswizardry.com/2012/07/shoot-to-kill-css-selector-intent/)

A couple of quick points that are important to remember:

* Abstract common design patterns (see 'Abstractions?' section below)
* Avoid making your components too brittle/tightly coupled  
e.g. don't set dimensional properties such as `width` (components should fill whatever space they are placed within)
* Layout properties (i.e. properties that affect layout, such as width/height/position/left/right/float etc) should be "opt-in" via a modifier (e.g. `.block__element--modifier` => `.module__img--left`). That way your module can be moved to any page, any section, any where and not break as the dimensions of the module should stretch to fit its container. 'Layout' properties break that fundamental requirement so the dev should specifically set those via opt-in modifiers.
* Make sure your selectors are as specific as they need to be  
e.g. A good way to know if you should create a specific class or target the element directly is to ask yourself the question: "am I selecting this because it’s a `ul` inside of `.header` or because it is my site’s main nav?"

###Some BEM naming guidelines
####Blocks
When building a 'component', the block name should be a succinct name for the component. 

For example, if you have a component called 'Related Links' then give it a block name of `related-links`.

When working with 'components' try to avoid the following conventions `<div class="component__related-links">` or `<div class="component component__related-links">`. 

The reason being: if you do have styles that are reusable within multiple components then internalise those styles like so...

```sass
%component-border {
    border: 1px solid red;
}

.related-links {
    @extend %component-border;

    // other styles specific to related links
}
```

...that way components avoid the issue where the naming convention of `component__something` forces the `something` component to inherit styles it might not necessarily need. If that situation occurred then you'd either have to go back and change the naming convention for all components OR for this one component change the naming convention to just `something` so it didn't inherit styles from `component` (which would be terrible for consistency and maintainability). 

Don't do it. Keep component names unique and use `@extend` to allow you to pass through styles you want.

####Elements
An element is a container within a block and can never appear outside of a container with the block name on. For example a title element on the related links component:

```html
<div class="related-links"></div>
<div class="related-links__title"></div>
```
**WRONG** - An element should never appear outside of it's block. 

```html
<div class="related-links">
    <div class="related-links__title"></div>
</div>
```
**GOOD** - The title element on the related-links block is contained inside it.

####Block elements vs blocks within blocks
They are both valid, but each should be used differently. The key is that a block is entirely self contained and can live and work on it's own correctly. An element is intrinsically related to it's block and can't work without it. So use an element if it only ever appears in it's containing block and use a block within a block if it's a further abstraction outside of the original component/block (something that is used in many places).

Simple Block element example
```html
<div class="related-links">
    <div class="related-links__title"></div>
    <a href="/some-link" class="related-links__link"></div>
</div>
```

Block within block (complex-link inside related-link)
```html
<div class="related-links">
    <div class="related-links__title"></div>
    <a href="/some-link" class="complex-link">
        <img src="some-img.jpg" class="complex-link__image"/>
        <div class="complex-link__text"></div>
    </div>
</div>
```

The examples are a bit contrived and the semantics not the best but hopefully you get the idea.

####Elements within elements from the same block
There are no strict rules around this, but generally it indicates poor naming/design and implies that the block itself is too complex by having a hierarchy that's too big. Instead of: 
```html
<div class="related-links">
    <div class="related-links__title"></div>
    <a href="/some-link" class="related-links__link">
        <h3 class="related-links__link__title"></h3>
    </a>
</div>
```

consider (block within block):
```html
<div class="related-links">
    <div class="related-links__title"></div>
    <a href="/some-link" class="complex-link">
        <h3 class="complex-link__title"></h3>
    </a>
</div>
```

The rules around blocks being separate entities still apply here. This route encourages creation of smaller blocks which are easier to reuse instead of one large block which does too much. If/when a pattern is established between this and another block, the 'related-links-link' block element could then be renamed to something like 'text-link' easily by removing the link between the two blocks from the similar block names.

The first example is valid, but it's difficult to draw a clear line between when it's ok and when the block is too complex, so use your best judgement.

#### Modifiers
Modifiers should only be applied to an element if the block or element class already exists on the element. If you find yourself wanting to do this, drop the modifier and just create a new block or element. Modifiers indicate a change from what is provided by the block/element by default.
```html
<div class="related-links--large"></div>
<!--WRONG-->
```

```html
<div class="related-links related-links--large"></div>
<!--CORRECT-->
```

```html
<div class="related-links">
    <div class="related-links__title--emphasise"></div>
</div>
<!--WRONG-->
```

```html
<div class="related-links">
    <div class="related-links__title related-links__title--emphasise"></div>
</div>
<!--CORRECT-->
```

Multiple modifiers on a block/element are perfectly valid. So for example, the following is ok:
```html
<div class="related-links related-links--large related-links--left"></div>
```

##Padding, margin, borders
Unless there is a good reason otherwise, where either a padding, margin or a border gives the desired results, white space between elements should be defined with a margin. 

An example of when padding is appropriate is if you wanted to create white space to help make the hit state on a link bigger. In that instance you would use padding on the link instead of a margin.

###Margin direction
We should aim to create margins in one direction as per advice from Harry Roberts (http://csswizardry.com/2012/06/single-direction-margin-declarations/). 

As for vertical spacing we should create margins as a **margin-top** instead of margin-bottom. The reason behind this is mainly to do with compatibility with older browsers. 

For example when creating a vertical list of items, we often want a consistent amount of spacing between items. So the first and last elements would need to have their margin removed.

If we were to use margin-bottom, we would need to do the following:

```css
.list-item {
    margin-bottom: 4px;
}

list-item:last-child {
   margin-bottom: 0;
}
```

or

```css
.list-item:not(:last-child) {
    margin-bottom: 4px;
}
```

Both of these are not good solutions as they rely on the usage of CSS3 selectors which aren't necessarily needed but more importantly aren't supported as widely as alternative solutions. 

Instead consider `margin-top`:

```css
.list-item + .list-item {
    margin-top: 4px;
}
```

Sibling selectors (adjacent sibling in this case) are CSS 2.1 so this will be more widely supported (this includes IE8). 

This solution also has the benefit of not having to undo a style which is preferable because it removes a potential specificity problem. The adjacent selector does out right have a higher specificity, but not at the cost of unsetting a style which is better.

If you do have to remove a margin-top then you can still use first-child which is also a CSS 2.1 selector.

As for left and right margin, I don't think we have a clear reason to use either for the time being, so use margin-right where either are valid in the name of consistency. 

##Abstractions?

The key to having a more modular code base is: 'abstraction' - taking a common design pattern and abstracting it into a self contained re-usable component.

Two prolific 'abstractors' are Nicole Sullivan (see: the ['media object'](http://www.stubbornella.org/content/2010/06/25/the-media-object-saves-hundreds-of-lines-of-code/)) and Harry Roberts (see: the ['island object'](http://csswizardry.com/2011/10/the-island-object/) and ['nav abstraction'](http://csswizardry.com/2011/09/the-nav-abstraction/)).

One thing to try and avoid is becoming too specific with your abstractions. Don't make the mistake of abstracting to a level so granular that you end up with one liners like the following...

```css
.abs {
	position: absolute;
}
```

...this might seem like a good idea ("I can now make any element have a position of absolute!") but this just causes code bloat and what's referred to as *class'itis*.

It's OK if you have 'similar' code inside different abstractions. The idea for an abstraction means it should be completely re-usable in another project. Make sure your abstractions can be picked up and stuck any where! This is where using BEM style selectors is very important as they allow this to be possible (e.g. element selectors such as `div > p` limit a component's ability to be moved as they become too tightly coupled to their current HTML environment).

When creating abstractions try to not set widths (or dimensions) on them! This can limit their re-usability. For example, if you have a box that you abstract into a separate class and currently it sits inside of a small side column and you decide you want to move it to the main content area - you ideally just want to cut and paste the HTML and have the styles be flexible enough that the box will now adapt to their new environment. If you had set a width on that box then you would have to change the width again once you moved it to the wider main content area (or have to create a new class to handle the new environment - not very flexible is it). Flexibility is key!

Harry Roberts [slides from 'Front-Trends 2012'](https://speakerdeck.com/u/csswizardry/p/breaking-good-habits) mention **The 4 '-ility's**:

* Maintainability
* Flexibility
* Extensibility
* Predictability

...these are important points to think about when creating your abstractions.

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

Implement a 'mobile first' approach which means you'll use a base stylesheet which displays the site as cleanly as possible on mobile devices. Then use Media Queries to load additional CSS at specific 'break points' that work well for the design.

Your media queries should not target current mobile/tablet screen dimensions - because in the future there may be other devices released with different dimensions that you haven't catered for. If your designs are fluid and are set to look good at specific break-points then your site should look good and be usable on pretty much any device.

##Resets

Don't use CSS resets. 

You end up writing more code to work around the lack of styling than you would have written to work around one or two browser 'quirks'.

Do use Nicolas Gallagher's [normalise.css](https://github.com/necolas/normalize.css) - *and feel free to add any modifications required*. The reason you should use 'normalise.css' is because it doesn't take a 'scorched earth' approach (i.e. wipes out all browser styles), it simply tries to normalise certain native styles (as well as try to fix some known quirky browser bugs) 

##Browser support

The only browser that you really need to be concerned about is Internet Explorer.

To work around some of the quirks in IE's rendering you should use Microsoft's Conditional Comments, but NOT to load in additional style sheets just for IE. 

For example, do **NOT** do this...

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

...this helps keep your CSS component code together and more easily maintainable.

What you should also hopefully find is when implementing a more modular and object-oriented (BEM) style approach to your CSS structure then the typical IE related bugs should be less apparent.

##Sprites

The idea behind sprites is pretty straight forward. Take all the little icons or miscellaneous imagery and stick them in one single image file, like so…

![](http://f.cl.ly/items/1Z3M010W210t25130P2h/apple.jpg)

…now you have helped towards your performance goal of '[reducing HTTP requests](http://developer.yahoo.com/performance/rules.html#num_http)' because instead of having (using the above image as an example) 20 different images, meaning 20 separate HTTP requests, you now have just one HTTP request.

You could argue the overall size of the image sprite can become very large and some pages might not use all the icons/images within the sprite, but by that point the sprite would have been loaded and cached by the browser and so that argument becomes redundant.

##Responsive Design

The principle solution to making your site design responsive is to not set widths on anything. Depending on the design of your site the layouts will need a width set - for example a two column layout needs to have a width set on each column (you can't avoid that), but any components you build shouldn't have a width set on them.

Anything you do set a width on should be in a flexible unit (either ems or percentages).

Here is an example of how you can handle this using the two column layout example above…

* add two `<div>`'s that will be your columns
* add a width to both columns in pixels
* float the columns so they sit next to each other
* now convert the pixels into percentages using the following algorithm `target / context = result`.

So for example, if you set the width of the left `div` to be 150px and its parent element happened to be a `<div>` with a width of 960px, then I would calculate the width of my left column like so: 

`150 / 960 = .15625`. 

But with percentages you wouldn't just set the width to `.15625%` you need to move the decimal place over by two places so it becomes `15.625%`. 

`target / context = result` is *THE* algorithm for responsive design!

Note: if you're calculating em values (e.g. converting font sizing from pixels to em) then the algorithm is the same but instead of the parent element being your 'context', you use the parent font size as the 'context', and whatever the resulting value, you don't move the decimal point. These types of calculations become much easier when using the REM unit. So rather than using the parent font size to base your calculation, you instead use the `<html>` element's font size instead (which depending on what you set should be approximately 16px).

##Responsive Images

To help your images scale appropriately along with your responsive design you can set the `max-width` property to be 100% which means the image will never be larger than its container but can happily resize/scale downwards on smaller screens…

```css
img {
    max-width: 100%;
}
```

##What about containing elements with unknown widths?

Normally what you see in web design is the main website content is wrapped in an element such as a `<div>` with a set `max-width` of (for example) 960px (so it fits onto a range of devices without requiring a horizontal scrollbar to appear). We use `max-width` instead of `width` for the same reason we use it for our responsive images, so it never gets larger than it should but can happily resize downwards depending on the size of the device viewing it.

But how do we convert 960px into a responsive unit? We know the algorithm for responsive design is `target / context = result` but we have no container in this instance (unless we count the `<body>` tag)? But then what's the width of the `<body>` tag? We don't know because it'll be different on every device!

Well there is one way to get 960px into a responsive unit and that is to take into account that the default font size on nearly all browsers is 16px. If you set `font-size: 100%` onto the `<body>` element then this equates to a font size of 16px.

This allows us to calculate the 960px `max-width` as 60em! 

By doing: `960 / 16 = 60` (again it's that `target / context = result` algorithm!)

We can now set our wrapper element like so (usually a good idea to include a comment that explicitly states the calculation)…

```css
.container {
    max-width: 60em; /* 960 / 16 */
}
```

## JavaScript Components
Use specific classes when you want to provide JS-only hooks for a component. These can be used to provide a JS-enhanced UI or to abstract other JS behaviours. 

Note: if you have a component you want to enhance with JS then you should ideally be setting the hook using the `id` HTML attribute (as it's more efficient for the JavaScript engine to access these elements using the host method `getElementById`).

**Pattern**

```
js-action-name
```

**Examples**

```
js-submit
js-action-save
js-ui-collapsible
js-ui-dropdown
js-ui-dropdown--control
js-ui-dropdown--menu
js-ui-carousel
```
