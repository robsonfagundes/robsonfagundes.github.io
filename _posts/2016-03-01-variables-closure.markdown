---
layout: post
title:  "JavaScript Variables - Closures."
date:   2016-03-01 11:59:11
categories: javascript variables
tags: featured
image: /assets/article_images/maxresdefault.jpg
---

JavaScript Variables - Global variables can be made local (private) with closures.
----------------  

Global variables can be made local (private) with closures. A closure normally occurs when one function is declared inside the body of another, and the inner function references local variables of the outer function. 

At runtime, when the outer function is executed, then a closure is formed, consisting of the interior function code and references to any variables in the scope of the outer function that the closure needs.

###### Example:

```javascript

let sayYourName = function(name) {
    let msg = "\nHello " + name + ". You're welcome!";
    let showMessage = function() {
        console.log( msg );
    };

    showMessage();
};

sayYourName("GitHub User");    // Hello GitHub User. You're welcome!

```


