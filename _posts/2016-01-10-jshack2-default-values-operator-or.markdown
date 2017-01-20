---
layout: post
title:  "JS Hack - Default value with logical operator ||"
date:   2016-01-10 13:30:05
categories: javascript
tags: featured
image: /assets/article_images/machine.jpg
---

JS Hack - Default value with logical operator || 
----------------  

In this post I will share another useful hack to use in JavaScript. 
When you have an object that needs default values when no parameter is passed to it, you can use the logical operator ||. 
To check whether the valid constructor parameters will be used or whether a default value will be used, for example:

```javascript

function User(name, age) {
   this.name = name || "New Dev.";
   this.age = age || 18;
}

var user1 = new User();
console.log(user1.name); // New Dev.
console.log(user1.age); // 18

var user2 = new User("Brendan Eich", 55);
console.log(user2.name); // Brendan Eich
console.log(user2.age); // 55

```
