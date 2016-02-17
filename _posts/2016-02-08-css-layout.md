---
layout: post
title:  "Intro to CSS layout"
date:   2016-02-08
categories: front end development
tags: [Web Development]
---

Using max-width instead of width will improve the browser's handling of small windows. This is important when making a site usable on mobile. 

{% highlight css %}
@media screen and (min-width: 480px) {
    body {
        background-color: lightgreen;
    }
}
{% endhighlight %}

## The box model

The original box model behavior was eventually considered unintuitive, so a new CSS property called `box-sizing` was created.
For example, some authors want all elements on their pages to always work this way. so most developer put the following 
CSS on their pages: 

{% highlight css %}
* {
  -webkit-box-sizing: border-box;
     -moz-box-sizing: border-box;
          box-sizing: border-box;
}

{% endhighlight %}

## Media queries

"Responsive Design" is the strategy of making a site that "responds to the browser and device that it 
is being shown on ... by looking awesome no matter what. 

{% highlight css %}
@media screen and (min-width:600px) {
  nav {
    float: left;
    width: 25%;
  }
  section {
    margin-left: 25%;
  }
}
@media screen and (max-width:599px) {
  nav li {
    display: inline;
  }
}
{% endhighlight %}

## Column

There is new set of CSS properties that let you easily make multi-column text. 

{% highlight css %}
.three-column {
  padding: 1em;
  -moz-column-count: 3;
  -moz-column-gap: 1em;
  -webkit-column-count: 3;
  -webkit-column-gap: 1em;
  column-count: 3;
  column-gap: 1em;
}
{% endhighlight %}


## flexbox

The new flexbox layout mode is poised to redefined how we do layouts in CSS.

{% highlight css %}
.container {
  display: -webkit-flex;
  display: flex;
}
.initial {
  -webkit-flex: initial;
          flex: initial;
  width: 200px;
  min-width: 100px;
}
.none {
  -webkit-flex: none;
          flex: none;
  width: 200px;
}
.flex1 {
  -webkit-flex: 1;
          flex: 1;
}
.flex2 {
  -webkit-flex: 2;
          flex: 2;
}
{% endhighlight %}


## CSS framework

Because CSS layout is so tricky, there are css frameworks out there to help make it easier. Here are 
a few if you want ot check them out. Using a framework is only a good idea if the framework really does 
what you need your site to do. 

* Foundation
* Pure.css
* Bootstrap
* Semantic UI
* blueprint