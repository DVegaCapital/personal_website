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

{% highlight html %}
<button onclick="sayHI()">Say HI</button>
<button onclick="unknownCmd()">Send unknown command</button>
<button onclick="stop()">Stop worker</button>
<output id="result"></output>

<script>
  function sayHI() {
    worker.postMessage({'cmd': 'start', 'msg': 'Hi'});
  }

  function stop() {
    // worker.terminate() from this script would also stop the worker.
    worker.postMessage({'cmd': 'stop', 'msg': 'Bye'});
  }

  function unknownCmd() {
    worker.postMessage({'cmd': 'foobard', 'msg': '???'});
  }

  var worker = new Worker('task.js');

  worker.addEventListener('message', function(e) {
    document.getElementById('result').textContent = e.data;
  }, false);
</script>
{% endhighlight %}

and typical javascript code like this one: 

{% highlight javascript %}
self.addEventListener('message', function(e) {
  var data = e.data;
  switch (data.cmd) {
    case 'start':
      self.postMessage('WORKER STARTED: ' + data.msg);
      break;
    case 'stop':
      self.postMessage('WORKER STOPPED: ' + data.msg +
                       '. (buttons will no longer work)');
      self.close(); // Terminates the worker.
      break;
    default:
      self.postMessage('Unknown command: ' + data.msg);
  };
}, false);
{% endhighlight %}

## Transferrable objects

One thing we must notice is that when passing these data using ```postMessage()```, a copy is made and pass to. Considering a 
situation if you are passing 500 MB file, there 's a noticeable overhead in getting that file between the worker and the main 
thread. 

To reduce the latency due to "copy" transfer, web worker introduce <strong>Transferable Objects</strong>. 

With transfterable objects, data is transferred from one context to another. the original objects from calling context is no longer 
available once transfer to the new context. It zero-copy, which vastly improves the performance of sending data to a worker. 












