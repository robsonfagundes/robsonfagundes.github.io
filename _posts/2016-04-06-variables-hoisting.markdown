---
layout: post
title:  "JavaScript Variables - Hoisting."
date:   2016-03-01 11:59:11
categories: javascript variables
tags: featured
image: /assets/article_images/hosting.jpg
---

JavaScript Variables - Hoisting.
----------------  

Formerly in languages like C, functions or procedures were used to split a program, but there was a problem: declarations should always be at the front, functions at the beginning of the program solved the problem for a while, since all functions and variables were declared before To be used, so that there were no errors of reference. 

"Why read the code from the bottom up, not from top to bottom?" 

Making things more user-friendly, we can now place the definitions anywhere in the code and use them even before they really are defined. What happens now is that compilers or even runtime languages read through the program to know what functions and variables you have declared in the code. After that, the actual execution happens and he already knows where each thing is. 

JavaScript does just that, what we call Hoisting.

###### Example:

```javascript

let a, foo = 'bar';
let bar = function() {
	let foo = 'foo';
	console.log('local: ' + foo);
};
bar();
console.log('global: ' + foo);

// local: foo
// global: bar

```


