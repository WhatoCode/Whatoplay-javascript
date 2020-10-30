# Whatoplay Javascript Style Guide

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

_A mostly reasonable approach to JavasScript guided by AirBnb_

> **Note**: this guide assumes you are using [Babel](https://babeljs.io/).

# Table of Contents

## Types

## Trivia

##### Pros and cons of `async` / `await` or generators

The polyfill for these contructs generates very large code.
This may change once all our supported browsers support ES6 natively.

Using async/await vs promise

```javascript
function sample() {
	return new Promise((resolve, reject) => {
		setTimeOut(() => {
			console.log("Hello world");
			resolve(true);
		}, 4000);
	});
}

sample().then((result) => console.log(result)); // using promise

(async () => {
	const result = await sample();

	return result;
})(); // using async await with IIFE
```

Code transpiled from babel

```javascript
"use strict";

function asyncGeneratorStep(gen, resolve, reject, _next, _throw, key, arg) {
	try {
		var info = gen[key](arg);
		var value = info.value;
	} catch (error) {
		reject(error);
		return;
	}
	if (info.done) {
		resolve(value);
	} else {
		Promise.resolve(value).then(_next, _throw);
	}
}

function _asyncToGenerator(fn) {
	return function () {
		var self = this,
			args = arguments;
		return new Promise(function (resolve, reject) {
			var gen = fn.apply(self, args);
			function _next(value) {
				asyncGeneratorStep(
					gen,
					resolve,
					reject,
					_next,
					_throw,
					"next",
					value
				);
			}
			function _throw(err) {
				asyncGeneratorStep(
					gen,
					resolve,
					reject,
					_next,
					_throw,
					"throw",
					err
				);
			}
			_next(undefined);
		});
	};
}

function sample() {
	return new Promise((resolve, reject) => {
		setTimeOut(() => {
			console.log("Hello world");
			resolve(true);
		}, 4000);
	});
}

sample().then((result) => console.log(result)); // using promise

_asyncToGenerator(function* () {
	const result = yield sample();
	return result;
})(); // using async await with IIFE
```

## Destructuring

-   Use object destructuring when accessing and using multiple properties of an object.

    > Why? Destructuring saves you from creating temporary references for those properties, and from repetitive access of the object. Repeating object access creates more repetitive code, requires more reading, and creates more opportunities for mistakes. Destructuring objects also provides a single site of definition of the object structure that is used in the block, rather than requiring reading the entire block to determine what is used.

```javascript
// bad
function getFullName(user) {
	const firstName = user.firstName;
	const lastName = user.lastName;

	return `${firstName} ${lastName}`;
}

// good
function getFullName(user) {
	const { firstName, lastName } = user;
	return `${firstName} ${lastName}`;
}

// best
function getFullName({ firstName, lastName }) {
	return `${firstName} ${lastName}`;
}
```

-   Use array destructuring.

```javascript
const arr = [1, 2, 3, 4];

// bad
const first = arr[0];
const second = arr[1];

// good
const [first, second] = arr;
```

-   Use object destructuring for multiple return values, not array destructuring.

    > Why? You can add new properties over time or change the order of things without breaking call sites.

```javascript
// bad
function processInput(input) {
	// then a miracle occurs
	return [left, right, top, bottom];
}

// the caller needs to think about the order of return data
const [left, __, top] = processInput(input);

// good
function processInput(input) {
	// then a miracle occurs
	return { left, right, top, bottom };
}

// the caller selects only the data they need
const { left, top } = processInput(input);
```
