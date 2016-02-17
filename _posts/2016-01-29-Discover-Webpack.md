---
layout: post
title:  "Discover Webpack workflow (with express)"
date:   2016-01-29
categories: front end development
tags: [Web Development]
---

Webpack now becomes the most populated words in front end community. we can combine webpack with gulp
to a powerful front end project framework. so let us start from scratch to see how to setup the workflow


## Project configuration file

here we use package.json as our deployment and project configuration file. 
To help us automate some processes, we filled the scripts with most used command: 

{% highlight python %}
{
    ...
    "scripts": {
        "start": "./node_modules/.bin/babel-node server", 
        "build": "rimraf dist && set NODE_ENV=production && webpack --config ./webpack.production.config.js --progress --profile --colors",
        "test": ...
     }, 
     ...
}
{% endhighlight %}

## Express server

Express is great framework to handle the requests from the browser if you want to use Node server as
development tool. 

*server.js*

{% highlight python %}
var express = require('express');
var path = require('path');

var app = express();

var isProduction = process.env.NODE_ENV === 'production';
var port = isProduction ? process.env.PORT : 3000;
var publicPath = path.resolve(__dirname, 'public');

// We point to our static assets
app.use(express.static(publicPath));

// And run the server
app.listen(port, function () {
  console.log('Server running on port ' + port);
});
{% endhighlight %}

## Workflow

Let us first configure a webpack for our development workflow: 

*webpack.config.js*
{% highlight python %}
var Webpack = require('webpack');
var path = require('path');
var nodeModulesPath = path.resolve(__dirname, 'node_modules');
var buildPath = path.resolve(__dirname, 'public', 'build');
var mainPath = path.resolve(__dirname, 'app', 'main.js');

var config = {

  // Makes sure errors in console map to the correct file
  // and line number
  devtool: 'eval',
  entry: [

    // For hot style updates
    'webpack/hot/dev-server',

    // The script refreshing the browser on none hot updates
    'webpack-dev-server/client?http://localhost:8080',

    // Our application
    mainPath],
  output: {

    // We need to give Webpack a path. It does not actually need it,
    // because files are kept in memory in webpack-dev-server, but an
    // error will occur if nothing is specified. We use the buildPath
    // as that points to where the files will eventually be bundled
    // in production
    path: buildPath,
    filename: 'bundle.js',

    // Everything related to Webpack should go through a build path,
    // localhost:3000/build. That makes proxying easier to handle
    publicPath: '/build/'
  },
  module: {

    loaders: [

    // I highly recommend using the babel-loader as it gives you
    // ES6/7 syntax and JSX transpiling out of the box
    {
      test: /\.js$/,
      loader: 'babel',
      exclude: [nodeModulesPath]
    },

    // Let us also add the style-loader and css-loader, which you can
    // expand with less-loader etc.
    {
      test: /\.css$/,
      loader: 'style!css'
    }

    ]
  },

  // We have to manually add the Hot Replacement plugin when running
  // from Node
  plugins: [new Webpack.HotModuleReplacementPlugin()]
};

module.exports = config;
{% endhighlight %}