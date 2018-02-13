---
layout: post
title: Equality, inequality and coercion in JavaScript 
tags: JavaScript coercion type conversion variables
---

In programming, a _primitive_ data type is the representation of data which is not an object and has no methods.

In JavaScript, there are 6 types of primitive data types: string, number, boolean, null, undefined and symbol(new in ES6). Most of the time, a primitive value is represented directly at the lowest level of the language implementation. All the primitive type of values are **immutable** (cannot be changed). The other built-in type in JavaScript is _object_.

> One more thing to note here is JavaScript has typed values not typed variables, so when we are comparing variables we are actually comparing values, not the actual variables.

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

> It may not be obvious but JavaScript coercion always result in scalar primitive values like _string_, _number_, or _boolean_. it will never result in complex value like _object_  or _function_.

Let's take a very classical example.

{% highlight javascript%}
    console.log(return 42 == '42');
{% endhighlight %} 

So, what do you think will will be logged in console?

Let's analyze the above code, what were are trying to do is comparing a number to string, and since 
