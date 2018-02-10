---
layout: post
title: Equality, inequality and coercion in JavaScript 
tags: JavaScript coercion type conversion variables
---

In programming, a _primitive_ data type is the representation of data which is not an object and has no methods.

In JavaScript, there are 6 types of primitive data types: string, number, boolean, null, undefined and symbol(new in ES6). Most of the time, a primitive value is represented directly at the lowest level of the language implementation. The other built-in type in JavaScript is _object_.

All the primitive type of values are **immutable** (cannot be changed).

One more thing to note here is "JavaScript has typed values not typed variables", so when we are comparing variables we are actually comparing values, not the actual variables.

Comparing values in JavaScript falls in two categories equality and inequality. Regardless of the type of values, we are comparing, the result is always strictly _boolean_.



###### Little information about coercion

Coercion is basically converting a value type to another value type. A simple example of this would be.

 {% highlight javascript %}
    var a = '12'; // a string
    var b = Number(a); //simple coercion

    console.log(a); // '12'
    console.log(b); // 12
 {% endhighlight %}

So here we are converting a string to number using _Number()_ an inbuilt function.

There are two types of coercion in JavaScript _implicit_ and _explicit_. Explicit coercion as the name suggests is done by us in code like in above example when we converted a _string_ value to a _number_ value whereas implicit conversion is when type conversion happens as more of a non-obvious side effect of some other operation.

There is a general misconception about **coercion** that sometimes it can produce some surprising results, fortunately, that is not true.
If we try to understand the underlying behaviour of the concept it will never surprise us.  
