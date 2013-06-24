---
layout: post
title: "Parsing HTML with DOMDocument and DOMXpath::query"
date: 2012-06-26 12:00
comments: true
categories: [php, DOMDocument, DOMXpath]
published: true
---
The other day I needed to do some **html scraping** to trim out some repeated data stuck inside nested `div`s and produce a simplified array of said data.  My first port of call was SimpleXML which I have used many times.  However this time, the son of a bitch just wouldn't work with me and kept on throwing up parsing errors.  I lost my patience with it and decided to give **`DomDocument`** and **`DOMXpath`** a go which I'd heard of but never used.

All of the examples I could find showed basic parsing of html, e.g. grabbing all `a` elements on a page, or grabbing all the child `book` elements in a bookstore XML document.  I needed to grab all the `div`s with a certain class then further parse their html to pull out bits of data.  Here's the html I'm working with for this example:

``` html
<html>
    <body>
        <h1>Foo</h1>
        <div id="content">
            <div class="foo">
                <div><img class="fooimage" src="http://foo.com/bar.png" /></div>
                <p class="description">Foo bar</p>
            </div>
            <div class="foo">
                <div><img class="fooimage" src="http://foo.com/baz.png" /></div>
                <p class="description">Baz bat</p>
            </div>
        </div>
    </body>
</html>
```

## Using DOMXPath::query to extract html data

I want an array of image src attribute values along with the value for the `p` elements.  The basic method of doing this is to query for the `div.foo` elements using `DOMXPath::query` which will return a `DOMNodeList`.  The list can then be iterated over which will produce a `DOMNode` every iteration.  Once the `DOMNode` for the `div.foo` element has been obtained, we need to then query again using `DOMXPath::query` but crucially pass the node as the `$contextnode` 2nd parameter, this will make the query relative to the `div.foo` node and allow us to grab the child `img` element's `src` attribute, and also the description from the `p` element.

Here's the test script:

{% gist 2996220 %}

Which gives the desired result:

``` php
Array
(
    [0] => Array
        (
            [image_src] => http://foo.com/bar.png
            [desc] => Foo bar
        )

    [1] => Array
        (
            [image_src] => http://foo.com/baz.png
            [desc] => Baz bat
        )

)
```