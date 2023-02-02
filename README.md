# JavaScript Arrays & Array Methods

## Goals

- Review JavaScript array basics
- Compare `.map()` and `.forEach()`
- Understand how to use `.filter()`, `.sort()` and `.reduce()`
- Get familiar with MDN documentation

## JS Array Basics

Arrays in JavaScript are the collection of choice for homogenous groups of data.
They are similar to Java's List/ArrayList collections, in that they are
inherently dynamic in size. They grow and shrink automatically to fit their
contents. Many of the properties and methods of JavaScript arrays should feel
familiar coming from Java.

```js
// create an array with an array literal
const myArr = [1, 2, 3];
// use array constructor to create another array
const anotherArr = new Array();

// access elements by zero-based indices
console.log(myArr[0]); // 1
console.log(anotherArr[0]); // undefined, no error thrown if accessing non-existent array items
// new ES2022 feature, the `.at()` method, allows access to negative indices (from end of array)
console.log(myArr.at(-1)); // 3

// add items to the end of an array
myArr.push(4);
anotherArr.push('hello');
// add items to the beginning of an array
myArr.unshift(0);
// remove the last item
myArr.pop();
// remove the first item
myArr.shift();

// access length property
console.log(myArr.length);
```

Technically, JavaScript arrays can hold mixed types.

```js
const arr = [
	'hello',
	1,
	() => console.log('it me'),
	{ firstName: 'Gordon', lastName: 'Saribudak' },
	[3, 4, 5],
];
```

But as so many things with JavaScript, just because we can, doesn't mean we
should. Arrays should hold homogenous groups of data.

Arrays, like all reference data types, are stored in heap memory. That means
variables pointing to arrays point to memory addresses, and don't hold the
actual value of the array.

**💡 What is different about JavaScript arrays compared to Java?**

### Efficiency of Arrays

We use the
[idea of "Big O"](https://www.freecodecamp.org/news/big-o-notation-why-it-matters-and-why-it-doesnt-1674cfa8a23c/)
as a shorthand for discussing the efficiency of algorithms. "Big O" describes
the worst-case time complexity of an algorithm, i.e., how the number of steps
the algorithm needs to perform scales when the size of the input grows
arbitrarily large and approaches infinity. When we consider which data
structures to use for our programs, we should have an idea of how efficient the
data structure is at the access, insertion, deletion, and search operations that
we commonly use them for.

#### [Array Big O](https://www.bigocheatsheet.com/)

| Access | Insertion | Deletion | Search |
| ------ | --------- | -------- | ------ |
| O(1)   | O(N)      | O(N)     | O(N)   |

Arrays are really fast at accessing elements if you know the index of the item
you need. Think of the indices as keys of an "array object," and the element
stored there as values. If you have the element's index, looking up the element
is an operation with constant worst-case time complexity -- the best Big O
possible.

Arrays are relatively slow at operations other than access. Search is slow
because you need to traverse the entire array to find the item you're looking
for. In the worst case scenario, if the item is not in the array or at the very
end, you would need to traverse the entire array in your search, giving us a
linear worst-case time complexity.

**💡 Are there ways to optimize search in arrays?**

Insertion and deletion are also slow operations because when items are inserted
or deleted, all the other items after the insertion/deletion need to have their
indices updated. In the worst-case scenario, if we insert/delete something at
the beginning of an array, every other element in the array will needs its index
updated, thus the linear worst-case time complexity.

**💡 What data structures, if any, are more efficient at insertion/deletion?**

## Array Methods

We can use loops to manually iterate through arrays and do just about anything
we need to do. But JavaScript has built-in array methods that help us perform
many of those same actions more concisely and declaratively.

Just like the Java 8 Stream API, these array methods take functions as
arguments. What we termed a "lambda" function in Java is usually called a
"callback" function in JavaScript. We'll use inline ES6 arrow functions in our
examples today, but you can also reference externally-defined functions or use
function declaration syntax.

### [Array.prototype.forEach()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)

Very similar to the Java Stream `.forEach()` method, the JavaScript array
`.forEach()` method is basically a built-in for-loop. It allows us to iterate
through an array and perform side effects. `.forEach()` has no return value.

Like all the other higher-order array methods we'll see today, `.forEach()`
takes a callback function as its argument. The parameters of this callback
function are predefined by JavaScript, just like they are in Java. The first and
only required parameter for the callback function is the element at the current
iteration through the array.

```js
const nums = [1, 2, 3, 4, 5];

nums.forEach((num) => {
	console.log(num);
});

// 1, 2, 3, 4, 5
```

While technically we can modify the elements and store them in a new array, as
you can see below, it's a bit messy.

```js
const nums = [1, 2, 3, 4, 5];
const doubles = [];

nums.forEach((num) => {
	const double = num * 2;
	doubles.push(double);
});
```

If you need to _transform_ the elements in your array, a cleaner choice is
`.map()`.

### [Array.prototype.map()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

When we talk about mapping, we generally mean taking one set of values and
creating a one-to-one correspondence with something else, such as when we map
routes to controllers in our backends. The `.map()` method is generally used to
transform data in an array, in a one-to-one manner. The callback function passed
to it executes once for every element in the array, and stores the return value
of the callback in the new array.

A cleaner implementation of creating a new doubles array from the previous
example can be achieved with `.map()`:

```js
const nums = [1, 2, 3, 4, 5];
const doubles = nums.map((num) => {
	return num * 2;
});

console.log(doubles); // 2, 4, 6, 8, 10
```

Notice how the original array remains un-mutated.

Using single-line arrow notation with an implicit return, we can make our code
even more concise:

```js
const doubles = nums.map((num) => num * 2);
```

One of the hallmarks of `.map()` is that it will return something for every
element in the array; the returned array will always have the same length as the
original array. But one of the most common pitfalls with mapping is forgetting a
return value from your callback function.

```js
const doubles = nums.map((num) => {
	num * 2;
});

console.log(doubles); // undefined, undefined, undefined, undefined, undefined
```

The logic in the callback function is usually the hardest part to grasp.
Basically, these higher-order methods have predefined function parameters for
the callbacks that we can see if we check out the
[documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map).
We see that we can access values other than just the element at the current
iteration, such as the index and the array itself.

It's up to us to write the logic that goes in the body of these callbacks, to
transform the data as ended for our application requirements.

**💡 How does the JavaScript `.map()` method compare to the Java Stream
method?**

**💡 When have we commonly used `.map()` in the Java training?**

### [Array.prototype.filter()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)

Filter, much like map, executes the provided callback function once for every
element in the array. Unlike map, filter keeps elements with a true (or truthy)
return value, and discards elements with a false (or falsey) return value from
the returned array.

With filter, you can expect the length of the return array to differ from the
length of the original array, based on how many items meet the return filter.

```js
// this keeps all the numbers that have a remainder that is not zero after being divided by 2
const odds = nums.filter((num) => num % 2 !== 0);

// can be written more concisely, taking advantage of truthiness of non-zero values:
const odds = nums.filter((num) => num % 2);
```

**💡 What are the advantages of using `.filter()` over a for-loop?**

### [Array.prototype.sort()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

Some computer scientists devote their careers to studying sorting algorithms.
Luckily for us, both Java and JavaScript have sorting methods built right in.
Different implementations of JavaScript implement different underlying sorting
algorithms. (Check out [this blog post](https://v8.dev/blog/array-sort) on
Chromium's V8 implementation if you want to dive deeper!)

Calling `.sort()` on a JavaScript array has no return value, and it mutates the
original array.

Let's see what happens if we call `.sort()` without passing a callback function.

```js
const words = ['hello', 'world', 'it', 'me'];
words.sort();
console.log(words); // 'hello', 'it', 'me', 'world'
```

What about an array of numbers?

```js
const moreNums = [3, -1, 100, 2345, -9];
moreNums.sort();
console.log(moreNums); // -1, -9, 100, 2345, 3
```

**💡 How did `.sort()` sort those numbers? Why do you think that is?**

Another [fun little quirk of JavaScript](https://i.redd.it/2m8yjg5irvl51.jpg).

As it turns out, the sort method does require a callback function to sort values
that are non-strings in a sensible way.

This callback function is a bit different than the ones we've seen so far. It
still gets executed on each item, but it is a compare function. The compare
function takes two parameters -- conventionally called `a` and `b` -- which
represent two elements from the array being looked at for comparison.

The compare function's return value helps the `.sort()` method know whether to
keep the original order of the items, to move a after b, or to move a before b.

| Return value | Sort order          |
| ------------ | ------------------- |
| 0            | Keep original order |
| Negative     | Move a before b     |
| Positive     | Move a after b      |

With that in mind, and assuming we're trying to sort our elements in ascending
order (least to greatest), let's try writing a compare function to use for our
sorting problem:

```js
function compare(a, b) {
	if (a === b) {
		return 0;
	} else if (a < b) {
		return -1;
	} else if (a > b) {
		return 1;
	}
}

const moreNums = [3, -1, 100, 2345, -9];
moreNums.sort(compare);
console.log(moreNums); // -9, -1, 3, 100, 2345
```

Ah, much better. We can actually write that more concisely as well, given the
fact we want to return a negative number when a is less than b, a positive
number when a is greater than b, and 0 if they're equal.

```js
moreNums.sort((a, b) => a - b);
console.log(moreNums); // -9, -1, 3, 100, 2345
```

**💡 How should the compare function change if we need to sort in descending
order?**

**💡 Imagine we're sorting an array of people objects in ascending order by
their `age` key. What might the compare function look like then?**

### [Array.prototype.reduce()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

In programming, a reducer is a function that boils a list of values down to one
value. The reduce method works a lot like that, executing a callback function
once for every element in the array, and accumulating the results to a single
value. That single value can be a number, a string, an object, and even another
array.

A really common use case (and great entry point) for this method is to find the
sum of an array of numbers. The reduce method actually takes up to two arguments
-- a reducer callback function that takes at least two parameters (an
accumulator and the current element), and an optional second argument, the
initial value of the accumulator.

```js
const moreNums = [3, -1, 100, 2345, -9];

const sum = moreNums.reduce((sum, currentValue) => {
	return sum + currentValue;
}, 0);

console.log(sum); // 2456
```

Taking advantage of single-line implicit returns, as well as the fact that if no
initial value is provided as the second argument, the first array element is
used as the initial value, we can greatly simplify the code:

```js
const sum = moreNums.reduce((sum, curVal) => sum + curVal);
```

Due to the reducer function and the accumulator value, `.reduce()` is the most
powerful and flexible of all the array methods.

We can use `.reduce()` to map:

```js
const doubles = moreNums.reduce((acc, curVal) => {
	const double = curVal * 2;
	acc.push(double);
	// the accumulator must be returned after each iteration
	return acc;
}, []);
```

We can use `.reduce()` to filter:

```js
const odds = moreNums.reduce((acc, curVal) => {
	if (curVal % 2) {
		acc.push(curVal);
	}
	return acc;
}, []);
```

We can use `.reduce()` to return any data type we want from an array iteration,
such as an object that counts the occurrences of all the letters in a sentence.

```js
const mySentence = 'Hello world it me';
const letterCounts = mySentence.split('').reduce((acc, curVal) => {
	// check if key for current letter is in the accumulator
	if (acc[curVal]) {
		// if so, just increment value of its count
		acc[curVal]++;
	} else {
		// if not, add it as a key and set its value to 1
		acc[curVal] = 1;
	}

	// be sure to return accumulator for next iteration
	return acc;
}, {});

console.log(letterCounts);
// {" ": 3, H: 1, d: 1, e : 2, i : 1, l : 3, m : 1, o : 2, r : 1, t : 1, w : 1}
```

**💡 What makes `.reduce()` able to replace many of the other array methods?**

## Conclusion

There are many other array methods to know and love in JavaScript. Like the Java
Stream API, these array methods give us a shortcut for iterating through and
manipulating data in a more declarative and often cleaner way.

You don't need to memorize any of these methods -- that's what MDN's fantastic
JavaScript documentation is for. With time and continued practice, as well as
learning about their parallels in other programming languages, they'll start to
feel like familiar tools in your toolkit.

## Attribution

The datasets in the data folder are courtesy of
the[ Art Institute of Chicago API](https://api.artic.edu/docs/) and
[Vega datasets](https://github.com/vega/vega/blob/main/docs/data/us-state-capitals.json).
