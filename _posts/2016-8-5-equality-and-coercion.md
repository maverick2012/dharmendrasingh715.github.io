---
layout: post
title: Equality, inequality and coercion in JavaScript 
tags: JavaScript coercion type conversion variables
---

In programming, a _primitive_ data type is the representation of data which is not an object and has no methods.

In JavaScript, there are 6 types of primitive data types: string, number, boolean, null, undefined and symbol(new in ES6). Most of the time, a primitive value is represented directly at the lowest level of the language implementation. All the primitive type of values are **immutable** (cannot be changed). The other built-in type in JavaScript is _object_.

> One more thing to note here is JavaScript has typed values not typed variables, so when we are comparing variables we are actually comparing values, not the actual variables.

Comparing values in JavaScript falls in two categories equality and inequality. Regardless of the type of values, we are comparing, the result is always strictly _boolean_.


##### Little information about coercion

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

Let's take an example.

{% highlight javascript%}
    console.log(return 42 + '');
{% endhighlight %} 

So, what do you think will will be logged in console?

Let's analyze the above code. What we are trying to do here is add a number to an empty string. The **+** operator will try to add the number with the empty string, however, the empty string is not a number so it will insist on concatenation (adding two strings) which as _(hidden) side effect_ will convert _number 42_ to its _string_ equivalent _"42"_. So, the console will log _"42"_ as a result.

We can get the similar result by explicitly converting a number to a string using _string(....)_ function. The terms _implicit_, _explicit_ and _hidden effects_ are relative because if we know what **42 + ""** is doing, and we are intentionally doing it, then this operation is sufficiently explicit for us. 


##### Truthy and Falsy

When a _non-boolean_ value is coerced to _boolean_, It will always result in either _true_ or _false_, so depending on the result we can say the value has _truthy_ nature if the result is true and _falsy_ when the result is false. Here is list of all values which have either _truthy_ or _falsy_ nature.

###### Falsy values

1. "" (Empty string)
2. 0,-0, NaN (Invalid number)
3. null, undefined
4. false

###### Truthy values

Any value which is not _falsy_ is _truthy_. Here are some examples of those:

* "Hello"
* 42
* true
* [], [1,2,3] (arrays)
* {}, {a:2,b:3} (objects)
* function foo() {..} (functions)


##### Equality

There are four operators to check equality between values, ==, != , ===, and !== . The **!** are symmatric "_not equal_" version of their  counterparts. _Non equality_ should not be counfused with _inequality_.

The main difference between == and === is understood as that == check for value equality, and === checks for value and type equality. However correct way to characterize them is that == checks for values equality with coercion allowed and === checks for value equality with coercion not allowed so it is often called  _"strict equality"_.

Let's take an example:

{% highlight javascript%}
    var a = "42";
    var b = 42;

    console.log(a == b); // true
    console.log(a === b); // false

{% endhighlight %} 

In the first comparison JS notices that the types do not match, so it goes through an ordered series of steps to coerce one or both values to a different type until the types match, where then a simple value equality can be checked. In the second case since coercion is not allowed so value conversion fails and results in false.

There are some edge cases in use of == which we need to remember otherwise == is a very powerful tool which helps us write code.

* If  either value in comparison can be a boolean value avoid == use === instead.
* If either value in comparison can be one of these values: 0,"", or [] (empty array), avoid using == use === instead.
* In all other case we are safe to use == .

> In case of comparing non-primitive values _objects_ and _arrays_, rules may defer for == and === because these values are actually held by reference so these operators only check if the reference matches, not anything about the underlying values.

For example, _arrays_ are by default coerced to _strings_ by simply joining all the values with commas ( , ) in between. You might think that two _arrays_ with the same  contents would be == equal, but they're not:

{% highlight javascript%}
    var a = [1,2,3];
    var b = [1,2,3];
    var c = "1,2,3";
    
    console.log(a == c); // true
    console.log(b == c); // true
    console.log(a == b); // false

{% endhighlight %}

###### Inequality

The < , > , <= , and >= operators are used for inequality, also known as "relational comparison." Typically they will be used with ordinally comparable values like
_numbers_. It's easy to understand that 3 < 4 . But JavaScript _string_ values can also be compared for inequality, using typical alphabetic rules ( "bar" < "foo" ).

What about coercion? Similar rules as == comparison (though not exactly identical!) apply to the inequality operators. Notably, there are no "strict inequality" operators that would disallow coercion the same way === "strict equality" does.

Consider:

{% highlight javascript%}
    var a = 41;
    var b = "42";
    var c = "43";

    console.log(a < b); // true
    console.log(b < c); // true

{% endhighlight %}

What happens here? If both values in the < comparison are _strings_, as it is with b < c, the comparison is made alphabetically like a dictionary. But if one or both is not a _string_ , as it is with a < b , then both values are coerced to be _numbers_, and a typical numeric comparison occurs.

The biggest gotcha we may run into here with comparisons between potentially different value types, since there are no "strict inequality" forms to use, it is when one of the values cannot be made into a valid number, such as:

{% highlight javascript%}
    var a = 42;
    var b = "foo";

    console.log(a < b); // false
    console.log(a > b); // false
    console.log(a == b); // false

{% endhighlight %}

How can all three of those comparisons be false ? Because the b value is being coerced to the "invalid number value" NaN in the < and > comparisons, and NaN is neither greater-than nor less-than any other value.

The == comparison fails for a different reason. a == b could fail if it's interpreted either as 42 == NaN or "42" == "foo", as we explained earlier, the former is the case.


##### Summary:

This is just a short overview of coercion and comparison (equality and inequality) in JavaScript. for more information read [here|https://developer.mozilla.org/bm/docs/Web/JavaScript/Equality_comparisons_and_sameness] and [here|https://github.com/getify/You-Dont-Know-JS/blob/master/types%20%26%20grammar/ch4.md]


This blog article is based on material in book "You don't know js" by [@YDKJS|https://twitter.com/@YDKJS], used under [CC BY|http://creativecommons.org/licenses/by-nc-nd/4.0/].
