---
layout: post
title:  "Creating restful CRUD app with Angularjs $resource"
date:   2016-02-03
categories: front end development
tags: [Web Development]
---

Most single page applications involve CRUD operations. If you are trying to build CRUD operations using AngularJS, then 
you can leverage the power of `$resource`

### Getting Started

`$resource` expects a classic RESTful backend. here I design the REST endpoints as following: 
    
* GET -----> Return all the entries;
* POST -----> New entry created;
* GET :id ------> Return single entry;
* PUT :id ------> updates an existing entry;
* DELETE :id ------> Deletes existing entry;


## How DOes $resource Work?

TO use `$resource` inside our service we need to declare a dependency on `$resource`. The next step is calling the 
`$resource()` function with your REST endpoints. 

{% highlight python %}
angular.module('myApp.services').factory('Entry', function($resource) {
  return $resource('/api/entries/:id'); // Note the full endpoint address
});
{% endhighlight %}

The result of the function call is a resource class object which has the following five methods by default:

1. get()
2. query()
3. save()
4. remove()
5. delete()

{% highlight python %}
angular.module('myApp.controllers',[]);

angular.module('myApp.controllers').controller('ResourceController',function($scope, Entry) {
  var entry = Entry.get({ id: $scope.id }, function() {
    console.log(entry);
  }); // get() returns a single entry

  var entries = Entry.query(function() {
    console.log(entries);
  }); //query() returns all the entries

  $scope.entry = new Entry(); //You can instantiate resource class

  $scope.entry.data = 'some data';

  Entry.save($scope.entry, function() {
    //data saved. do something here.
  }); //saves an entry. Assuming $scope.entry is the Entry object  
});
{% endhighlight %}

.......to be continue