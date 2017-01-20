---
layout: post
title:  "JavaScript Variables - Instantiation using an IIFE"
date:   2016-02-18 09:12:19
categories: javascript variables
tags: featured
image: /assets/article_images/iMacCode.png
---

JavaScript Variables - Instantiation using an IIFE 
----------------  

IIFE (Immediately-Invoked Function Expression), is a function that executes immediately after it is defined.

There are several places we can use an IIFE, but the most common is when we want to create a scope in JavaScript so that the variables defined within the function do not pollute the global scope (variables defined in the window).

###### Example:

```javascript

(function () {
  'use strict'

  var hello = 'iS2JS'
  console.log(hello) // 'I love JavaScript'
})()

console.log(hello) // ReferenceError: hello is not defined

```

###### Let's see how a variable can receive a value from an IIFE:

```javascript

var count = (function () {
  var num = 0;

  return function () {
    return num ++;
  }
})();

count() // 0
count() // 1
count() // 2
count() // 3
count() // 4

```

###### How to pass one variable per parameter to the IIFE:
In the second parentheses, where we invoke the function, we can pass any parameter as if it were any other function.

```javascript

(function (msg) {
  console.log(msg)
})("hi there what's up")

// Log 'hi there what's up'

```
These variables sent will be treated as scope variables of that function and will only be accessible within it, which allows IIFE to use variables in a script without interfering with others.
