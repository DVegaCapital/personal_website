---
layout: post
title:  "Getting Started with Stylus"
date:   2016-01-29
categories: front end development
tags: [css]
---

Stylus is a very popular css preprocessor, with lots features like extending class, creating mixins, importing spreadsheets and setting variables. Let's check out the basics of stylus and get into css programming. 

## Installation and Setup

Stylus is hosted in github and can be easily installed using NPM package manager:

{% highlight python %}
$ npm install stylus -g
{% endhighlight %}

Stylus files are given .styl extension, can be placed anywhere in the project. 
Here I listed some useful usages options here. *--watch* option will automate compile your stylus to css, *--compress* option will compress css output. Also in project you can integrate stylus with *gulp* 

{% highlight python %}
$ stylus stylus/main.styl --out /css --compress

$ stylus --watch stylus/main.styl
{% endhighlight %}


## Stlyus Syntax Basics

Let's review some basics syntax. 
It show features like

  - variable setting
  - mixin calls
  - loop iterator
  - extend existing class

{% highlight python %}
/* Set a basic variable */
base-font-size = 12px

/* Set a variable based on result of mixin call */
body-background = invert(#ccc)

/* Set some basic rules */
body
    color #333
    background #fff

/* Nest rules */
nav
    margin 10px

    ul
        list-style-type none

        > li
            display inline-block

            &.current
                background-color lightblue

/* Use calculated values */
div.column
    margin-right (grid-spacing / 2)
    margin-top (grid-spacing * 2)

/* Use an already-set value for this element */
div.center-column
    width 200px
    margin-left -(@width / 2)

/* Get styles for this element from the result of a mixin */
.promo
    apply-promo-styles()
    apply-width-center(400px)

/* Iterate over a loop! */
table
    for row in 1 2 3 4 5
        tr:nth-child({row})
            height: 10px * row

    /* or.... */
    for row in (1..5)
        tr:nth-child({row})
            height: 10px * row


/* Import another Stylus stylesheet */
@import 'links.styl'

/* Extend an existing class */
.warning
    @extend .noticeBlock
{% endhighlight %}


## Using Mixins

Mixins are incredibly useful within CSS preprocessors for a number of reasons. They allow us to use logic to generate CSS but they also allow us to organize our CSS. 

{% highlight python %}
/* 
    Basic mixin which vendorizes a propery.  Usage:

    vendorize(box-sizing, border-box)
*/
vendorize(property, value)
    -webkit-{property} value
    -moz-{property} value
    -ms-{property} value
    {property} value
{% endhighlight %}


Check out the [Stylus github][github] for more info on how to get the most out of Stylus. 

[github]:      https://github.com/stylus/stylus


