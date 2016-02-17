---
layout: post
title:  "Javascript Concurrency : Web Workers"
date:   2016-02-13
categories: front end development
tags: [Web Development]
---

## The Problem: JavaScript Concurrency

Today I learned something about multi-threading in Javascript --- HTML5 Web Workers. I would like to see how to apply these
for better web performance. 

At the old days Javascript does not support multi-threading because the javascript interpreter in the browser is a 
single thread (AFAIK). Even Google Chrome will not let the web page's Javascript run concurrently because this would 
cause massive concurrency issues in existing web pages(like intermediate state viewed by different users). All 
Chrome does is separate multiple components (different tabs, plugins, etc) into separate processes. 

But now days in HTML5 Web Workers provide a simple means for web content to run scripts in background threads. This is 
real multi-threading situation. Non-blocking doesn't mean concurrency, Asynchronous events are processed after the current
executing script has yielded. 


## Web Workers API

{% highlight javascript %}
var worker = new Worker('task.js');
{% endhighlight %}

like other threading concept, Workers utilize thread-like message passing to achieve parallelism. 

```postMessage()``` we can start a worker by calling this method. 

{% highlight javascript %}
var worker = new Worker('task.js');

worker.addEventListener('message', function(e){
    console.log('Worker said: ', e.data);
}, false);

worker.postMessage('hello world');
{% endhighlight %}