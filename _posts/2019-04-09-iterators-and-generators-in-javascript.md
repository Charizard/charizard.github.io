---
layout: post
title: Iterators and Generators in Javascript
tags:
- iterators
- generators
- javascript
---

Iterators are one of the least used features in Javascript. But, when used correctly, it gives very powerful abilities.

### What is the Iterator pattern?

When an object responds to a `next()` function which produces the next value in a sequence every time it is called, then it follows the iterator pattern. The return value should also specify if the end of the sequence has been reached, so the return value of the function is not just the value but, an object with a `value` and a `done` properties.

Consider a function which generates a sequence of even numbers till 10. Which would look like,

{% highlight javascript %}
let EvenNumbers = function() {
  let value = 0;
  return {
    next: function() {
      if (value > 10) {
        return { done: true };
      }
      return { value: value + 2, done: false };
    }
  }
}
{% endhighlight %}

Now, this can be used like,

{% highlight javascript %}
let evenNumbers = new EvenNumbers();
evenNumbers.next().value  // 2
evenNumbers.next().value  // 4
evenNumbers.next().value  // 6
evenNumbers.next().value  // 8
evenNumbers.next().value  // 10
evenNumbers.next()        // { done: true }
{% endhighlight %}


The real power with this is the ability to store the state of the sequence and call `next()` to get the next number in the sequence.

Do you remember when I said that iterators are one of the lease used features in JavaScript? Well, I lied. All `for...of` loops use iterators to loop around. `Array` is one of the very widely used example of Iterators. Which means for us, we can loop around the above sequence using a `for...of` loop.

{% highlight javascript %}
let evenNumbers = new EvenNumbers();
for (var x of evenNumbers) {
  console.log(x);		// 2,4,6,8,10
}
{% endhighlight %}

### Now, what are Generators?
Generators are a syntactical sugar for writing custom iterators. They remove the need for writing careful code for explicitly maintaining the internal state of the iterator.

For example, the above `EvenNumbers` generator can be written as,

{% highlight javascript %}
function * evenNumbers(start = 0) {
  for (let i = start + 2; i < 10; i += 2) {
    yield i;
  }
}

let evenNumbersGenerator = evenNumbers();

evenNumbersGenerator.next().value 	// 2
evenNumbersGenerator.next().value 	// 4
evenNumbersGenerator.next().value 	// 6
evenNumbersGenerator.next().value 	// 8
evenNumbersGenerator.next().value 	// 10
evenNumbersGenerator.next()		 	// { done: true }

for (var x of evenNumbers()) {
  console.log(x);		// 2,4,6,8,10
}
{% endhighlight %}

If you compare the generator example with the iterator example, you will see that both of them achieve the same thing, but the generator uses very less code. `yield` takes care of returning the Object with `value` and the `done` flag. Once the function execution stops, the `{ done: true }` is returned as well.

### Generators vs Iterators
Generators have a huge advantage over the traditional iterators in many ways.

* No need to maintain internal state which leads to Lesser code.
* No need to return an object with value and done flags.
* Almost looks like a normal function.

Hope you would be very eager to start using generators in your project. Please comment how you are using / planning to use them below.
