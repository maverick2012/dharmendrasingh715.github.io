---
layout: post
title: Equality, inequality and coercion in JavaScript 
tags: JavaScript coercion type conversion variables
---

Comparing values in js falls in two type equality and inequality. Regardless of the type of values, we are comparing result is always strictly _boolean_.

######Coercion

Coercion is basically converting a value type to another value type. A simple example of this would be.

 {% highlight javascript %}
    var a = '12'; // a string
    var b = Number(a); //simple coercion

    console.log(a); // '12'
    console.log(b); // 12
 {% endhighlight %}

So here we are converting a string to number using _Number()_ an inbuilt function.
