---
layout: post
title: Javascript pass parameters by value (and by reference value) example
description: "Code snippet example showing javascript pass parameters by value (and reference). A deep explanation of what is
going on is present."
author: ama
permalink: /snippets/610d3e31f52006531138d835/javascript-pass-parameters-by-value-and-reference-example
published: true
categories: [snippets]
tags: [javascript, codever-snippets]
---

Still confused about how passing variables in Javascript functions works? So was I, until recently. It took me some effort to understand,
and I'd like to share my understanding with an example.

First try guessing the outcome of the following javascript snippet

## Input

```javascript
const number = 1983
const string = 'Adrian'
let obj1 = {
  value: 'obj1'
}
let obj2 = {
  value: 'obj2'
}
let obj3 = obj2;

let obj4 = ['a'];


function change(numberParam, stringParam, obj1Param, obj2Param, obj4Param) {
    console.log('\nSTART logs in function');

    numberParam = numberParam * 10;
    stringParam = 'Ionut';
    console.log('numberParam - ', numberParam);
    console.log('stringParam - ', stringParam);

    console.log('obj1Param.value in function before obj1Param = obj2Param assignment - ', obj1Param.value);
    obj1Param = obj2Param;
    console.log('obj1Param.value in function after obj1Param = obj2Param assignment - ', obj1Param.value);

    console.log('obj2Param.value in function before obj2Param.value change - ', obj2Param.value);
    obj2Param.value = 'changed'; // obj1Param.value = 'changed'; would yield the same result
    console.log('obj1Param.value in function after obj2Param.value change - ', obj1Param.value);
    console.log('obj2Param.value in function after obj2Param.value change - ', obj2Param.value);

    //obj4Param = ['b'];
    obj4Param.push('b');
    console.log('obj4Parma - ', obj4Param);

    console.log('END logs in function \n');
}

change(number, string, obj1, obj2, obj4);

console.log('number final - ', number);
console.log('string final - ', string);
console.log('obj1.value final - ', obj1.value);
console.log('obj2.value final - ', obj2.value);
console.log('obj3.value final - ', obj3.value);
console.log('obj4 final - ', obj4);
```

> Before you read the output I suggest you read the explanation of [Call by sharing](https://en.wikipedia.org/wiki/Evaluation_strategy#Call_by_sharing) on Wikipedia,
> which is the best explanation on the topic I found.

## Output

```javascript
START logs in function
numberParam -  19830
stringParam -  Ionut
obj1Param.value in function before obj1Param = obj2Param assignment -  obj1
obj1Param.value in function after obj1Param = obj2Param assignment -  obj2
obj2Param.value in function before obj2Param.value change -  obj2
obj1Param.value in function after obj2Param.value change -  changed
obj2Param.value in function after obj2Param.value change -  changed
obj4Parma -  ["b"]
END logs in function

number final -  1983
string final -  Adrian
obj1.value final -  obj1
obj2.value final -  changed
obj3.value final -  changed
obj4 final -  ["a"]
```

Ok, so what's going on?
- `number` and `string` primitives are "boxed"[^1]
 in `Number` and `String` objects[^2] before passing. **Boxed objects are always a copy of the value object**,
  hence new objects (Number and String) are created in memory with the same primitive values.
   In the function execution (scope) they get "unboxed", their value is changed and placed in the new memory space, but once
    the function is over the new space in memory is cleared, with the original remaining unaffected.
- a copy reference to `obj1` and `obj2` is passed to the function, pointing to the same address of the "original" objects in memory (**call by sharing**)[^3].
 With the `obj1Param = obj2Param` assignment in the function, both `obj1Param` and `obj2Param` to the original `obj2` object in memory, so when changing
 its property `obj2Param.value = 'changed'` it will also be visible outside of the function scope, after it is terminated.
 `obj1Param.value = 'changed'` would have had the same effect after the assignment.
- what about `obj4`? `obj4param` is also a copy reference to the `obj4` object (remember in Javascript arrays are objects),
but with the `obj4Param = ['b']` assignment it's pointing now to a newly created object (the `['b']` array object), which is visible only
in the function's scope and is destroyed when the function is over. Thus, it has no effect on the original object. On the other hand,
 a statement like `obj4param.push('b')` would have changed the original array and would display a ` ["a", "b"]` value.

[^1]: <https://en.wikipedia.org/wiki/Object_type_(object-oriented_programming)#Boxing>
[^2]: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects>
[^3]: <https://en.wikipedia.org/wiki/Evaluation_strategy#Call_by_sharinghttps://en.wikipedia.org/wiki/Evaluation_strategy#Call_by_sharing>

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="610d3e31f52006531138d835" %}
