<<<<<<< HEAD
# HTML Style Guide
=======
#HTML Style Guide

This is my own personal style guide for writing HTML. 

Feel free to take the bits you like and/or modify to your own style.

Here is what we'll cover:
>>>>>>> 4b3390062afb4d83c953522108997311a781656a

- Terminology
- Document Object Model (DOM)
- Style
- Comments
- Internet Explorer
- Example

##Terminology

For full description please [read this article](http://www.456bereastreet.com/archive/200508/html_tags_vs_elements_vs_attributes/)

Term                  | Example
--------------------- | -------------
'tag'                 | `<p>` or `<div>` (a tag is the start or ending of an 'element')
'element'             | `<p>this is an example of an 'element'</p>`
'attribute'           | `<img alt="alt is an attribute">`
'section'             | `<head>` or `<body>`
'comment'             | `<!-- this is a comment (hidden from the user) -->`
'conditional comment' | `<!--[if IE]> IE specific content goes here <![endif]-->`

##Document Object Model (DOM)

*The following is just a brief introduction to the DOM.  
I've included it here as it ties into the next section of this guide - which discusses our 'Style' of writing HTML…*

The DOM is an API which allows languages (such as JavaScript) to interact with the HTML page. If you have an element in the page, then a corresponding object is generated within the DOM to represent that element.

Interacting with the DOM via JavaScript is notoriously slow. An analogy used by many people is that of a bridge toll preventing you from crossing the bridge to the other side. On one side of the bridge is your JavaScript file and on the other side of the bridge is the DOM's representation of your HTML. For you to be able to cross the bridge you need to pay the toll. If you have a lot of work to do then it would make more sense to pay the toll once, cross the bridge, do everything you need to and come back. You wouldn't want to pay the toll multiple times for every little thing that needs to be done.

JavaScript uses the DOM and its API's to access different aspects of the HTML page so (because of the potential performance implications) the DOM's representation of the HTML page needs to be as simple as possible (because it'll make it easier for JavaScript to find what it's looking for and to make the changes it needs to make). This means your HTML needs to be as lean and simple as possible - which will result in a simpler DOM representation, and that in turn will result in more efficient and easier JavaScript interactions.

##Style

- Keep white space to a minimum. 
- Use 4 spaces for indentation
- Do not use XHTML closing syntax `<br />` - just use standard syntax `<br>` instead
- Lowercase all elements and attributes (it just looks so much neater).
- Indent main 'sections' - for example…  
    ```html
    <html>
        <head>
            <meta>
        </head>
        <body>
            Content
        </body>
    </html>
    ```
- Keep `<head>` section in the following order…  
    1. `<meta>` elements
    2. Any required scripts (e.g. [Modernizr](http://modernizr.com/) or the [HTML5 Shiv](https://github.com/aFarkas/html5shiv))
    3. `<title>` element
    4. Style sheets
- Keep HTML structure simple and not too deeply nested.
- Ensure the DOM is as small and efficient as possible. The semantics of sections/elements are important but don't wrap other elements such as a `<ul>` (which is typically used for a main navigation) in a `<nav>` element unless you really need to. Just use `<ul class="nav">` to represent a `<ul>` that is a navigation module.
- Avoid using HTML5 `section` and `article` elements (screen readers are a bit funny with those)
- Use ARIA roles and labels when appropriate
- Use HTML5 form controls when applicable to trigger the right keyboard on mobile (`url`, `email`…)

##Comments

Don't use comments at the end of elements. We understand the principle behind it (e.g. it can make it easier to find the end of a section) but it is just so god damn ugly.

##Internet Explorer

To work around some of the quirks in IE's rendering do use Microsoft's Conditional Comments, but NOT to load in additional style sheets just for IE.

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

...this helps keep your CSS modules together and more easily maintainable.

##Example

Here follows is a basic boilerplate of an HTML page… 

```html
<!doctype html>
<!--[if IE 8]><html class="ie8" dir="ltr" lang="en"><![endif]-->
<!--[if IE 9]><html class="ie9" dir="ltr" lang="en"><![endif]-->
<!--[if gt IE 9]><!--> <html dir="ltr" lang="en"> <!--<![endif]-->
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <title></title>
        <!--[if lt IE 9]>
        <script src="html5.js"></script>
        <![endif]-->
        <link rel="author" href="/humans.txt" type="text/plain">
        <link rel="stylesheet" href="mobile.css"  media="only screen and (min-width: 320px)">
        <link rel="stylesheet" href="tablet.css"  media="only screen and (min-width: 600px) and (max-width: 959px)">
        <link rel="stylesheet" href="desktop.css" media="only screen and (min-width: 960px)">
        <!--[if (lt IE 9) & (!IEMobile)]>
        <link rel="stylesheet" href="desktop.css">
        <![endif]-->
    </head>
    <body>
        <script data-main="Assets/Scripts/App/main" src="Assets/Scripts/require.js"></script>
    </body>
</html>
```
