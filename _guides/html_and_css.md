---
layout: default
title: 'HTML and CSS Basics'
---

# HTML and CSS Basics

As [part of the coursework](https://backend.turing.io/module2/intermission_work/sql) for the Back-End Engineer program at [Turing School of Software & Design](https://turing.io), I compiled the following reference guide to the basics of HTML and CSS. The information is largely derived from the [HTML](https://www.w3schools.com/html/default.asp) and [CSS](https://www.w3schools.com/css/default.asp) guides on [w3Schools](https://www.w3schools.com/), which I recommend as a great place to familiarize yourself with these foundational tools of web design. But if you'd like a more condensed summary, read on!

## HTML

1. What is HTML?

     HTML (Hyper Text Markup Language) is the programming language used to create, format, and display web pages. It consists of a series of elements, represented by tags, which are used by browsers to render the content of the pages. HTML pages, at their most basic, have an `HTML` element which contains a `head` element (containing meta-information like the page's `title`) and a `body` element which contains the visible elements of the web page. There can also be a `DOCTYPE` element at the very top of an HTML document declaring itself to be an HTML5 document (`<!DOCTYPE html>`).

1. What is an HTML element?

    An HTML element, represented by tags, labels a piece of content. It is not displayed by the browser directly but is used by the browser to render the web page. Tags are element names surrounded by angle brackets; they often come in pairs, with an opening/start tag and a closing/end tag which has a forward slash before the tag name. E.g., `<html>` and `</html>`. Those tags surround the content described by them; everything from the opening tag to the closing tag is considered to be that element. Elements can be nested within one another. Tags are case-insensitive but convention is to keep them lowercase.

1. What is an HTML attribute?

    HTML elements can have attributes, specified in their opening tag, are (frequently) name/value pairs which further describe an element. For example, link elements (`<a>`) can have an `href` attribute, the value of which is the URL which the link directs the user to, and image elements (`<img>`) can have a `src` attribute, the value of which is the URL/filename of the image to be displayed. Attributes are case-insensitive but it is recommended to keep them lower-case, and can be surrounded by quotation marks or not but it is recommended to put quotation marks around them (either single or double).

1. What is the difference between a class and an id? When would you use one vs. the other?

     Classes and ids are attributes which any HTML element can have, and are used to style those elements using CSS. The classes and ids allow CSS code to target which HTML elements inherit which style properties. Any number of HTML elements can share a class, while an id is unique to a single element. Elements can have multiple classes, and can have both classes and an id, but can have only one id. Classes would be used when more than one HTML element needs to share style properties, while an id would be used when one element needs to be styled uniquely or differently than others (perhaps in its class). An id can also be used to create a link to a specific location in an HTML page.

1. What HTML would you write to create a form for a new dog with a "name" and an "age"?

     The HTML for such a form would look something like this:
     ```html
     <h1>Submit a new dog</h1>

     <form action="some-other-page.html" method="post">
          Name of dog:<br>
          <input type="text" name="name" required><br>
          Age of dog:<br>
          <input type="text" name="age" required><br>
          <input type="submit" value="Submit Dog">
     </form>
     ```

1. What are semantic tags? When would you use them over a `div`?

     Semantic element tags were introduced in HTML5 to replace and standardize what developers were doing by giving specific id or class names to `<div>` elements to describe standard structural components of a web page. They function like `<div>` or `<span>` but are more descriptive, making it easier for search engines to index their contents and for users with disabilities to navigate. It would be preferable to use one over a `<div>` when the content being wrapped matches the definition of one of the semantic tags, but not when the element is only being used for presentational purposes (i.e., to modify the layout of a webpage) or if it stretches a definition.

1. Explanations of what the major HTML tags do and when you would use them:
    * `<h1>`, `<h2>`, etc.

      The headings tags `<h1>`, `<h2>`, `<h3>`, `<h4>`, `<h5>`, and `<h6>` are used to creater header elements from most significant or higher sections to least significant or lower, thus showing the document structure of a web page. They are often used by search engines to skim a webpage.

    * `<p>`

       The `<p>` tag defines a paragraph element, used to define a paragraph of text. By default, browsers remove extra line breaks and spaces from the content inside these tags. The `<br>` tag, an "empty tag" which needs has no closing tag, forces a line break without ending the paragraph. The `<pre>` tag for pre-formatted text can alternatively be used for text which should preserve spaces and line breaks (for example, from poems or code blocks). By default, browsers add a space before and after paragraph elements.

    * `<body>`

       The `<body>` tag defines the body element of an HTML page, which contains the content of the page (as opposed to the metadata in the `<head>` tag). It is where the text, images, input objects, and other information on the web page are defined.

    * `<a>` and the `href` attribute

       The `<a>` tag defines a hyperlink element which allows a user to click an element to navigate to another document. It has an `href` attribute (hypertext reference) which provides either an absolute path to another website (with the full URL, i.e. `http://www....`) or a relative path to a document on the same website (i.e., `cat_photo.png`). Whatever is between the tags becomes a clickable link, so it can be text, an image, or more. `<a>` also can have a `target` attribute specifying in which window or frame the link should open in, and a "title" attribute which takes a string as a value that describes what the link points to.

    * `<img>` and the `src` attribute

       The `<img>` tag describes an image element. It is an empty tag, meaning it has no closing tag. The "src" attribute takes a URL as its value, which points to the location of the image that should be displayed. Like `<a href="">`, the "src" attribute can take either an absolute path or a relative one. `<img>` also optionally takes "width" and "height" attributes with numbers which are the number of pixels across or down, respectively, which the image should be displayed as; and an "alt" attribute which provides an alternative text description of the image for users with disabilities or for whom the image won't load.

    * `<div>`

       The `<div>` tag is a block-level element (taking up all available left-to-right space) used to group other HTML elements together for formatting or organization purposes. Its inline-level corollary is `<span>`. Both `<div>` and `<span>` are non-semantic elements, in that they indicate nothing about the meaning or purpose of what is inside them, as opposed to semantic elements introduced in HTML5.

    * `<section>`

       `<section>` is one of many semantic element tags introduced in HTML5 to replace and standardize what developers were doing by giving specific id or class names to `<div>` elements to describe standard structural components of a web page. They function like `<div>` or `<span>` but are more descriptive, making it easier for search engines to index their contents and for users with disabilities to navigate. `<section>` describes a section of content on a web page, typically containing a header and information related to that header.

    * `<ul>`, `<ol>`, and `<li>`

       The `<ul>` and `<ol>` tags describe unordered-lists (i.e., bullet lists without numeric order) and ordered-lists (i.e, numbered, ordered lists), respectively, and `<li>` tags describe list items within the opening and closing `<ul>` or `<ol>` tags on those lists. `<ol>` takes and optional "type" attribute which changes what is used to order the list, e.g, numbers, lower-case letters, or lower-case roman numerals. An alternative list element is the description list, defined with the `<dl>` tag, within which there are description term elements described by `<dt>` tags, each with a description definition element, described by `<dd>` tags.

    * `<form>`

       The `<form>` tag defines a form element, which is not displayed but contains multiple `<input>` elements which would gather input from a user and be submitted together. It has an "action" attribute which defines where to send the data provided by the user using the various `<input>` elements within the form. It also has a "method" attribute with either "get" or "post" as its value which determines the method used to send the data to the location provided by its "action." `GET` has a 2048-character limit and sends the information by appending it to the URL, and should only be used for non-sensitive data. `POST` has no size limit and is not appended to the URL.

    * `<input>`

       The `<input>` tag (empty, needing no closing tag) defines an input element, the type of which varies depending on which value is given as its "type" attribute: `<input type="text">` would create a text box input element allowing the user to type input, `<input type="radio">` would create a radio-button input element allowing the user to select only one of a number choices, and `<input type="submit">` would create a submit button inoput element allowing the user to submit all the data in the `<form>` element. Every `<input>` element also needs a "name" attribute in order for its data to be sent when the form is submitted. `<input>` elements can be further organized using `<fieldset>` elements, which themselves can be named using the `<legend>` element within each.

## CSS

1. What is CSS?

     CSS (Cascading Style Sheets) in a programming language used to define the layout and style of HTML documents. It was created to separate out the style, layout, etc. from HTML so that those could be defined independently and in one place for multiple HTML files to reduce the time cost of modifying style as web pages grew to include several to hundreds of HTML documents. Thus, while CSS code can be embedded within an HTML document in its `<head>` element or as the value of an individual element's `style` attribute, the value of CSS is maximized when it is stored in `.css` documents which multiple HTML files can inherit style properties from.

1. What is a CSS selector? How do you use the ID selector? The class selector?

     A CSS selector is the first component in a CSS rule, and it selects which HTML elements the style rules will apply to. The most basic selector is an element selector, which selects all instances of a given element; e.g., `img { property: value; }`. Multiple elements can be styled at once by separating them with commas: `img, p, section { ... }`. An id selector would select only the element with a given id, specified by prepending an octothorpe symbol to it: `#thing-1 { property: value; other-property: value; }`.

     A class selector correspondingly selects all the HTML elements with a given class, specified by prepending a period: `.things-class { property: value; }`. The class selector can be combined with the element selector to select specific elements with a given class by given the element followed by a period and then the class: `img.things-class { property: value; }`. Finally, an asterisk can be used as a universal selector to select all HTML elements: `* { property: value; }`.

1. What are the three ways to include CSS in your HTML documents? What are the pros and cons of each?

     The three ways to include CSS in an HTML document are: A) to link an external `.css` file in the HTML document's `<head>` element with the `<link>` tag; B) to include it internally within the HTML document's `<head>` element within `<style>` tags; and C) to use inline CSS which places property/value pairs within an element as the value for its `style` attribute, like so: `<img src="..." style="width:550px;height:400px">`.

    External CSS is always preferred because it maintains the advantages for which CSS was created, separating style from content. Internal CSS within the `<head>` element allows for unique styling of a specific document while keeping it somewhat separate from the content in the `<body>` element, but that could be accomplished by linking multiple CSS files and having the last one (which takes precedence over the earlier ones) be unique to a page. Inline CSS erodes the purpose of CSS entirely and should be avoided, but it takes priority over all other stylesheets and thus can be used to override them in specific instances.

1. What is the Box Model, and what are its components?

     The Box Model is how CSS handles the layout and styling of individual HTML elements. It treats the element content itself as in the innermost content box (such as text or images), which is wrapped in a transparent padding box, outside of which is the border (if defined), outside of which is the transparent margin box. Content, padding, border, and margin can each be styled independently; content can be given a width and height, while the latter three can be given unique sizes on each side (top, right, bottom, and left,  in that order if using shorthand syntax).

     Each HTML element has all four "boxes", so to calculate an elements distance from an adjacent one the sizes of all the boxes need to be taken into account. Margins operate uniquely in that top and bottom margins of adjacent elements "collapse," meaning the larger of the two margins is used and the smaller is removed entirely. Content width and height and the sizes of the sides of margin, border, and padding can be defined as `auto` (default, lets the browser decide), in a given unit such as `px`, as a `%` of the containing block, or set to `inherit` the values of the parent block.
