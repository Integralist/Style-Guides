#HTML Style Guide

This is my own personal style guide for writing HTML. 

Feel free to take the bits you like and/or modify to your own style.

This isn't a 'righteous' guide. If there is anything here you don't like, then don't worry. I'm not forcing you to play along.

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
I've included it here as it ties into the next section of this guide - which discusses my 'Style' of writing HTML…*

The DOM is an API which allows languages (such as JavaScript) to interact with the HTML page. If you have an element in the page, then a corresponding object is generated within the DOM to represent that element.

The DOM is notoriously slow (in nearly all popular web browsers). Others have likened it to: two pieces of land ('DOM land' and 'JavaScript land') which are separated by a river and connected by a bridge. To get to 'DOM land' from 'JavaScript land' you need to "cross the bridge". But every time you cross the bridge you have to pay a toll. So if you're going into 'DOM land' you better get everything you need done before you come back, as you don't want to have to pay twice for something you forgot to do.

JavaScript uses the DOM to access different aspects of the HTML page and because of the potential performance implications, if you're using JavaScript to access the DOM, then the DOM's representation of the HTML page needs to be as simple as possible as it'll make it easier for JavaScript to find what it's looking for (or to make the changes it wants to make). This means your HTML needs to be as simple as possible - which will result in a simpler DOM, and that in turn will result in better/easier JavaScript access.

##Style

I like to keep white space to a minimum. 

I like to lowercase all elements and attributes (it just looks so much neater).

I like indenting main 'sections', for example… 

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

I like to keep my `<head>` section in the following order… 

1. `<meta>` elements
2. Any required scripts (e.g. [Modernizr](http://modernizr.com/) or the [HTML5 Shiv](https://github.com/aFarkas/html5shiv))
3. `<title>` element
4. Style sheets
5. Internet Explorer Conditional Comments

I also like to ensure the DOM is as small and efficient as possible. I know the semantics of sections/elements are important but I really hate the idea of wrapping a `<ul>` which is used for my main navigation in a `<nav>` element. I prefer to just use `<ul class="m-nav">` to represent a `<ul>` that is a navigation module.

Something else I like to do with HTML (which isn't strictly speaking 'HTML' but does relate somewhat) is to flush the content buffer to the browser as soon as possible. To do this I use a server-side language (in my case PHP) to flush the content manually. So what this means is throughout my HTML, at key points (and by 'key points' I mean any where there is a large chunk of HTML - e.g. a `<section>` or `<article>`) I'll stick in `<?php flush(); ?>`. This helps with a user's *perceived performance* of the page loading, because while the page is loading the server pushes content to the browser as quickly as possible rather than waiting for all the content to be loaded before sending it to the browser.

##Comments

I don't like using comments at the end of elements. I understand the principle behind it (e.g. it can make it easier to find the end of a section) but it is just so god damn ugly.

##Internet Explorer

To work around some of the quirks in IE's rendering I use Microsoft's Conditional Comments to load in additional style sheets just for IE.

Here are some Examples of how you can use these conditionals… 

```html
<! --[if lte IE 9]>
<link rel="stylesheet" href="Assets/Styles/OLD-IE.css">
<![endif]-->
```

or

```html
<! --[if IE 8]>
<link rel="stylesheet" href="Assets/Styles/IE8.css">
<![endif]-->

<! --[if IE 7]>
<link rel="stylesheet" href="Assets/Styles/IE7.css">
<![endif]-->
```

##Example

Here follows is a basic boilerplate of an HTML page I normally start out with… 

```html
<!doctype html>
<! --[if IE]><![endif]--><!-- Explanation: http://www.phpied.com/conditional-comments-block-downloads/ -->
<html lang="en" dir="ltr">
	<head>
		<meta charset="utf-8">
		<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=0">
    	<meta name="description" content="" />
    	<meta name="author" content="" />
		<!--[if lt IE 9]>
		<script src="Assets/Scripts/html5.js"></script>
		<![endif]-->
		<title>Mobile First (Desktop Second)</title>
		<link rel="author" href="humans.txt" type="text/plain">
		<link rel="stylesheet" href="Assets/Styles/base.css">
		<link rel="stylesheet" href="Assets/Styles/mobile.css"  media="only screen and (min-width: 320px)">
        <link rel="stylesheet" href="Assets/Styles/tablet.css"  media="only screen and (min-width: 480px) and (max-width: 959px)">
        <link rel="stylesheet" href="Assets/Styles/desktop.css" media="only screen and (min-width: 960px)">
		<!--[if lt IE 9]>
			<link rel="stylesheet" href="Assets/Styles/desktop.css">
		<![endif]-->
	</head>
	<body>
        <div class="l-container">
            <h1>Main Header</h1>
            <h2>Secondary Header</h2>
            <p>Below is a numbered list displaying some code related to known screen dimensions</p>
            <ol>
                <li>mobile <code>(min-width:320px)</code></li>
                <li>tablet <code>(min-width:480px) and (max-width:959px)</code></li>
                <li>desktop <code>(min-width:960px)</code></li>
            </ol>
    		<blockquote>
    			<p>Some mobile browsers, notably Safari iPhone, have a default layout viewport of around 850 to 1000 pixels — much larger than the device width. (Why? In order to accomodate desktop sites whose developers did not test on mobile. Such sites usually stretch to roughly that width.)</p>
    		</blockquote>
        </div>
		<script src="Assets/Scripts/mobile.js"></script>
	</body>
</html>
```

…one thing to point out is the `<meta name="viewport">` element. In the above example I have set `user-scalable=0` which effectively means (don't allow the user to zoom). Now I don't advise using that unless you have a really good reason to. The only time I've ever had to disable zooming is when building a touch based HTML5 Canvas game. I've kept it in the above example, but just be aware that that particular setting isn't necessary majority of the time.